## Why

Local development workflows for testing, linting, and benchmarking vary across machines and require manual Docker/Podman setup. Containerized scripts standardize this, support both runtimes, and use caching to speed up runs (e.g., 10s cargo test with cache hit) without compromising integrity.

## What Changes

- New `scripts/container-base.sh`: Shared lib for runtime (Docker/Podman), image (`rust:1.80-slim`), mounts, caching (target/, Cargo.reg).
- New `scripts/test-container.sh`: Run `cargo nextest --lib`, `cargo test`, pass args.
- New `scripts/lint-container.sh`: `cargo clippy`, `fmt --check`, `test --doc`.
- New `scripts/bench-container.sh`: `cargo bench message_bus --no-run`.
- README section: Usage, setup (podman machine), cache prune.

No breaking changes.

## Capabilities

### New Capabilities

- `containerized-devtools`: Standardized containerized execution of tests, lints, benches with Docker/Podman support and integrity-safe caching.

### Modified Capabilities

(None)

## Impact

- New `scripts/` dir (4 files).
- README.md + optional docs/scripts.md.
- Dependencies: Docker/Podman (opt-in).
- No runtime/code changes; devtools only.