## Context

The `scripts/container-base.sh` script has two bugs:
1. Line 34: `~` inside double-quoted volume mount does not expand to `$HOME`.
2. Line 46: `container_run "$@"` runs even when script is sourced (e.g., by other build scripts).

Reference: proposal.md

## Goals / Non-Goals

**Goals:**
- Correct sccache cache directory mounting in Docker.
- Prevent execution of `container_run` when script is sourced.

**Non-Goals:**
- Changes to container runtime or other scripts.
- New features.

## Decisions

1. **Tilde expansion fix**:
   - Replace `~` with `$HOME` in the volume mount string.
   - Rationale: Bash does not expand `~` inside double quotes; `$HOME` is explicit and portable.
   - Alternative: Remove quotes around path (risky for paths with spaces).

2. **Execution guard**:
   - Wrap `container_run "$@"` in `if [[ "${BASH_SOURCE[0]}" == "${0}" ]]; then ...; fi`.
   - Rationale: Standard idiom to detect direct invocation vs. sourcing (`BASH_SOURCE[0]` is caller script when sourced).
   - Alternative: Check `[[ $- == *i* ]]` (interactive), but less precise.

## Risks / Trade-offs

- [Risk: `$HOME` differs in container] → Use `$(pwd)` or fixed path if needed, but sccache typically uses `$HOME`.
- [Risk: Guard false negative] → Tested idiom widely used in bash scripts.