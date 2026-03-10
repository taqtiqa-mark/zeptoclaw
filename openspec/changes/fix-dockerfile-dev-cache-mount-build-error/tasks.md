## 1. Syntax Fix

- [x] 1.1 Dockerfile.dev: Move `--mount=` after `RUN` (before `mkdir src`).
- [ ] 1.2 Test syntax: `podman build -f Dockerfile.dev .` (fallback) or docker/podman-buildkit.

## 2. Script Clean

- [x] 2.1 lint-container.sh: Revert `--buildkit` flag (invalid).
- [ ] 2.2 Test: `./scripts/lint-container.sh` builds plain.

## 3. Fallback

- [x] 3.1 Dockerfile.dev: Comment `--mount=` lines `# BuildKit only`.
- [ ] 3.2 Test fallback: `podman build -f Dockerfile.dev .` succeeds.

## 4. sccache

- [ ] 4.1 Docs: README.md → sccache install + `--sccache` usage.
- [ ] 4.2 lint/test-container.sh: If `sccache --version`, add `--sccache`.

## 5. Polish

- [ ] 5.1 Write `.cargo/config.toml`: `registry-path = "/src/Cargo-registry"`.
- [ ] 5.2 container-base.sh: Auto `podman volume create zeptoclaw-{target,registry,benches}_cache` if missing.

## 6. Verify

- [ ] 6.1 `podman volume rm zeptoclaw-*`; `rm localhost/zeptodev`; `./scripts/lint-container.sh`.
- [ ] 6.2 `./scripts/test-container.sh lib` (check cache hit).
- [ ] 6.3 Edit src/lib.rs → relint timings (sccache win?).