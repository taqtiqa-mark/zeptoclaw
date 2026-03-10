## 1. Fix shift crash

- [x] 1.1 Edit `scripts/test-container.sh` line 7: replace `shift` with `shift || true`.

## 2. Add cargo-nextest install in all mode

- [x] 2.1 Edit `scripts/test-container.sh` line 18 `all)` case: insert `cargo install --locked cargo-nextest || true && ` before `cargo nextest run --lib`.

## 3. Verify fixes

- [x] 3.1 Test no crash: `./scripts/test-container.sh` (uses default `all`, no shift error).
- [x] 3.2 Test install: `./scripts/test-container.sh all` (confirms nextest runs).
- [x] 3.3 Test other modes: `./scripts/test-container.sh lib`, `integration` (no regressions).