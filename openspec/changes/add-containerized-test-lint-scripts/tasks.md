## 1. Create scripts directory and base library

- [x] 1.1 `mkdir scripts`
- [x] 1.2 Create `scripts/container-base.sh` per design: runtime detect, rust:1.80-slim, common mounts/cache, `container_run` fn.
- [x] 1.3 `chmod +x scripts/container-base.sh`

## 2. Implement test-container.sh

- [x] 2.1 Create `scripts/test-container.sh`: source base, parse arg (lib/integration/all/default), run `cargo nextest run --lib` or `cargo test`, passthru.
- [x] 2.2 `chmod +x scripts/test-container.sh`
- [x] 2.3 Verify: `./scripts/test-container.sh lib` succeeds.

## 3. Implement lint-container.sh

- [ ] 3.1 Create `scripts/lint-container.sh`: source base, run `cargo clippy --all-targets -- -D warnings && cargo fmt --all --check && cargo test --doc`.
- [ ] 3.2 `chmod +x scripts/lint-container.sh`
- [ ] 3.3 Verify: `./scripts/lint-container.sh` passes/runs linters.

## 4. Implement bench-container.sh

- [x] 4.1 Create `scripts/bench-container.sh`: source base, run `cargo bench --bench message_bus --no-run`.
- [x] 4.2 `chmod +x scripts/bench-container.sh`
- [x] 4.3 Verify: `./scripts/bench-container.sh` prints summary.

## 5. Test Podman support

- [ ] 5.1 `podman volume create target_cache registry_cache benches_cache` (doc).
- [ ] 5.2 `CONTAINER_RUNTIME=podman ./scripts/test-container.sh lib` succeeds.

## 6. Update documentation

- [ ] 6.1 Add README.md section: "Containerized Devtools", usage, podman setup, cache prune.
- [x] 6.2 Optional: docs/scripts.md.

## 7. Quality gates & Git hygiene

- [ ] 7.1 Run `cargo fmt -- --check`, `cargo clippy -- -D warnings` (local).
- [ ] 7.2 GH issue: `gh issue list --repo qhkm/zeptoclaw --state open` , create if none (chore, area:devtools, P2).
- [ ] 7.3 Create PR with `Closes #N`, present URL, wait CI/user merge.