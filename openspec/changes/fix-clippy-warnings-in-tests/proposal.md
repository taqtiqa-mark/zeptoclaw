## Why

Clippy warnings in test code indicate potential issues, inconsistencies, or style violations that could affect code quality and maintainability. Fixing them ensures the codebase adheres to Rust best practices, reduces noise in CI, and prepares for future upgrades like Rust edition 2024.

## What Changes

- Address approximately 55 Clippy warnings detected in test code across all targets (`cargo clippy --all-targets`).
- Apply fixes using safe, idiomatic Rust patterns without altering test behavior or logic.
- Update any necessary imports or patterns flagged by Clippy (e.g., unused variables, unnecessary unwraps in tests, redundant clones).
- No changes to production code; focus exclusively on test modules and integration/bench files.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This is a chore change focused on code hygiene in tests, not altering any spec-level requirements or behaviors.

## Impact

- Affected files: Test modules in `tests/`, `benches/`, `src/bin/`, and integration tests (e.g., `tests/integration.rs`, `benches/message_bus.rs`).
- Dependencies: None; uses existing Clippy lints and Rust stdlib.
- Systems: Improves CI reliability (e.g., `cargo clippy -- -D warnings` will pass cleanly); no runtime impact.