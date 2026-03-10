## Why

Fix two bugs in `scripts/test-container.sh`:
1. Line 7: `shift` with no args crashes under `set -e`.
2. Line 18: `all` mode invokes `cargo nextest` without prior installation (unlike `lib` mode).

Ensures reliable containerized testing.

## What Changes

- **scripts/test-container.sh:7**: Guard `shift` with `shift || true` or `[[ $# -gt 0 ]] && shift`.
- **scripts/test-container.sh:18**: Add `cargo install --locked cargo-nextest` before `cargo nextest --all` in `all` mode.

No breaking changes.

## Capabilities

### New Capabilities
(none)

### Modified Capabilities
(none - build/test script fixes; no spec-level changes)

## Impact

- Affected: Containerized tests (`scripts/test-container.sh lib|all`).
- Improves: Test reliability in CI/Docker.
- Dependencies: cargo-nextest (already used in lib mode).