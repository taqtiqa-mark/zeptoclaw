## Context

Podman 5.7 legacy builder fails on `--mount=type=cache` syntax in Dockerfile.dev RUN (sh treats as cmd).

## Goals / Non-Goals

**Goals:**
- Seamless PR builds: `./scripts/lint-container.sh` (auto-build + lint).
- Fast iterative/PR rebuilds (syntax fix + volumes/sccache).
- Runtime persistence (volumes + sccache).

**Non-Goals:**
- Docker-only; support podman rootless.

## Decisions

1. **Syntax Fix (Primary)**
   - Reposition `--mount=` immediately after `RUN` (before shell cmds).
   - Docker BuildKit OOTB; podman legacy → fallback.

2. **Podman BuildKit (Opt)**
   - Docs: `~/.config/containers/containers.conf [engine] buildkit = true` + restart podman.socket.
   - Revert lint-container.sh `--buildkit` (no flag).

3. **Fallback: Comment Mounts**
   - `# --mount=... (BuildKit only)` → plain `cargo build`.

4. **sccache Integration**
   - Lint/test scripts: Auto `--sccache` if host `sccache --version`.
   - Host setup/docs: curl install.sh + RUSTC_WRAPPER=sccache.

5. **Registry Polish**
   - `.cargo/config.toml`: `registry-path = "/src/Cargo-registry"`.

6. **Volumes/Build**
   - Auto `podman volume create zeptoclaw-*_cache`.
   - `--userns=keep-id`.

## Risks / Mitigations

- Podman conf: Docs + fallback safe.
- sccache: Auto-detect.
- Cold build: sccache/volumes mitigate.
- Volumes: Script auto-init.