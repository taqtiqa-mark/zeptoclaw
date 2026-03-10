## 1. Preparation

- [ ] 1.1 Run `cargo clippy --all-targets --message-format=json | jq '.[] | select(.message.spans[].file_name | contains("tests") or contains("benches") or contains("bin"))' > warnings.json` to capture test-specific warnings
- [x] 1.2 Parse warnings.json to group warnings by file (e.g., using jq or manual review); estimate ~55 warnings across tests/integration/benches
- [x] 1.3 Run `cargo test` as baseline to record passing test count and outcomes

## 2. Identify and Fix Warnings by Category

- [x] 2.1 Scan for and fix unused imports/variables (lint: unused_imports, unused_variables) in all test files; use `edit` tool for exact replacements
- [x] 2.2 Address needless returns or borrows (lint: needless_return, unnecessary_operation) in test functions; replace with direct expressions
- [ ] 2.3 Fix redundant clones or unwraps in tests (lint: redundant_clone, unwrap_used); prefer `clone().expect("test setup")` or `?` where applicable
- [x] 2.4 Handle type complexity or manual implementations (lint: manual_{retain,filter_map}) in integration tests; refactor to use iterator methods
- [x] 2.5 Resolve remaining lints (e.g., nonminimal_bool, option_map_or_none) in benches and bin tests; batch edits per file

## 3. File-Specific Fixes

- [x] 3.1 Review and fix `tests/integration.rs` (likely multiple warnings; read file, apply design patterns like expect for mocks)
- [x] 3.2 Fix `benches/message_bus.rs` (focus on benchmark-specific patterns; ensure no performance regressions)
- [x] 3.3 Address warnings in `src/bin/benchmark.rs` tests if present; verify with `cargo test --bin benchmark`
- [ ] 3.4 Scan and fix any other test modules (e.g., `src/tools/*_tests.rs` if applicable; use `find src tests benches -name "*.rs" | xargs grep -l "test"` to locate)

## 4. Verification and Cleanup

- [x] 4.1 After each batch (e.g., per category or file), run `cargo clippy --all-targets -- -D warnings` to confirm reduction in warnings
- [x] 4.2 Run `cargo test` and compare to baseline; ensure 100% pass rate with no new failures
- [x] 4.3 Apply `cargo fmt` to affected files for consistency
- [x] 4.4 Final full run: `cargo clippy --all-targets -- -D warnings` should output 0 warnings from test code
- [x] 4.5 Document any `#[allow]` additions with justifications in comments (rare, per design)

## 5. Git and Issue Tracking

- [x] 5.1 Commit changes with message "fix: resolve Clippy warnings in test code (closes #187)"
- [ ] 5.2 Create PR targeting main; include before/after Clippy output in description
- [ ] 5.3 Update AGENTS.md and CLAUDE.md with test count changes if applicable
- [ ] 5.4 Run quality gates: `cargo fmt -- --check`, `cargo clippy -- -D warnings`, `cargo nextest run --lib`, `cargo test --doc`