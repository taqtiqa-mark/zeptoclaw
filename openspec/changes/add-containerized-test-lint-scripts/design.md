## Context

No standardized containerized dev workflow; users run `docker run --rm -v .:/src rust cargo test` manually, no caching, Podman unsupported, slow repeats. Proposal adds scripts for test/lint/bench with caching/target mounts, Docker/Podman.

## Goals / Non-Goals

**Goals:**
- Single command: `./scripts/test-container.sh lib` runs containerized cargo nextest --lib with cache (~10s hit).
- Dual runtime: Docker/Podman via $CONTAINER_RUNTIME.
- Caching: target/Cargo.reg mounts/volumes for speed, safe for tests (read/write, no persistent DB).
- Fail-fast, arg passthru.

**Non-Goals:**
- Replace CI workflows.
- Support non-Rust cmds or custom images.
- Auto-clean cache or host setup (document).

## Decisions

1. **Base library (`scripts/container-base.sh`)**:
   - Detect runtime: $CONTAINER_RUNTIME (docker/podman) or 'docker'.
   - Image: `rust:1.80-slim` (matches edition 2021, minimal).
   - Common mounts: -v $(pwd):/src:rw ; cache volumes for /src/target, /src/Cargo-registry,/src/benches/target.
   - Functions: `container_run` wraps runtime run --rm -i -v ... cargo "$@".
   - Rationale: DRY, easy extend. Alt: Inline per script - rejected for duplication.

2. **Caching**:
   - Docker: --mount=type=cache,id=target,target=/src/target ; same for registry/benches.
   - Podman: --volume target_cache:/src/target:rw (pre-create podman volume create target_cache).
   - Script detects runtime, uses appropriate.
   - Rationale: Docker experimental cache mount (24+), Podman volumes standard. Alt: Host dir mount - pollutes host FS.
   - Integrity: Cache target ok (tests use temp files), cargo clean if corrupted via manual.

3. **Script cmds**:
   - test: nextest --lib ; test integration/all.
   - lint: clippy all-targets -D warnings ; fmt --all --check ; test --doc.
   - bench: bench message_bus --no-run.
   - Passthru args.
   - Rationale: Match AGENTS.md quality gates. Alt: Single script - rejected for simplicity.

4. **Docs**:
   - README section with usage, podman setup (`podman machine init`).
   - Cache prune cmds.
   - Rationale: Self-contained.

## Risks / Trade-offs

- [Cache corruption (stale deps)] → Mitigation: `cargo clean` before run or prune volumes.
- [Podman machine not running] → Mitigation: Document setup, fallback error.
- [Large image pull first run] → Mitigation: Use slim image, user pulls manually if needed.
- [Runtime not installed] → Mitigation: Fail with install instructions.

No migration (new scripts).

**Container-Only Gates Decision**:
 - Pre-push hygiene via scripts only (`lint-container.sh`, `test-container.sh lib`, `bench-container.sh`).
 - Rationale: Guarantees common env/results (rust:1.80 deps/caches), no host toolchain divergence. Host cargo optional for quick iter.

5. **Dockerfile.tests Integration**:
   - Base image enhancements from `Dockerfile.tests`: apt pkg-config libssl-dev, rustup clippy/rustfmt.
   - Script supports `IMAGE=zeptotest` (docker build -t zeptotest Dockerfile.tests).
   - Rationale: Pre-installs project deps (providers need openssl), clippy ready. Alt: Inline RUN in script - rejected for bloat.

6. **Optional sccache Support**:
   - `--sccache` flag: Install sccache, `export RUSTC_WRAPPER=sccache`, mount /root/.cache/sccache volume.
   - Rationale: 2-5x faster compiles on cache hit, integrity via hash verification. Volume per-project prefix.
   - Alt: Always-on - rejected (overhead for simple test runs).