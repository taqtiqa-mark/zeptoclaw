## Context

`scripts/test-container.sh` bugs:
1. Line 7: `shift` crashes under `set -euo pipefail` if no args post-parsing.
2. Line 18: `all` mode runs `cargo nextest --all` without `cargo install cargo-nextest`.

Reference: proposal.md

## Goals / Non-Goals

**Goals:**
- Prevent `shift` crash in argless invocation.
- Ensure `cargo-nextest` available in `all` mode.

**Non-Goals:**
- New test modes or Cargo changes.

## Decisions

1. **Shift guard**:
   - Use `shift 2>/dev/null || true` after arg check.
   - Rationale: Ignores shift fail silently; preserves `set -e`.
   - Alternative: `if [[ $# -gt 0 ]]; then shift; fi` (cleaner, no subshell).

2. **Nextest install**:
   - Add `cargo install --locked cargo-nextest` before `cargo nextest` in `all` branch.
   - Rationale: Matches `lib` mode; `--locked` for reproducibility.
   - Alternative: Global install (less portable).

## Risks / Trade-offs

- [Risk: install time overhead] → ~30s first-run, cacheable.
- [Risk: shift guard swallows errors] → Limited to post-args shift.