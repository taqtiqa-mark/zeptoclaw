## 1. Podman-only

- [x] 1.1 container-base.sh: Podman req, drop docker fallback.

## 2. Rootless ns (tutorial)

- [x] 2.1 container_run: --userns=keep-id.
- [x] 2.2 podman build calls: --userns=keep-id.

## 3. Dockerfile.dev

- [x] 3.1 Apt: --no-install-recommends.
- [x] 3.2 cargo-nextest=0.10.3.
- [x] 3.3 Add tutorial comments.

## 4. Verify

- [x] 4.1 ./lint-container.sh from root (build/run rw ok).
- [x] 4.2 ./test-container.sh all (no perms).
