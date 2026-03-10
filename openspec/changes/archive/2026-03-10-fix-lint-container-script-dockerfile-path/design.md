## Context

`scripts/lint-container.sh` line 15: `$RUNTIME build -f ../Dockerfile.dev ...` assumes cwd=scripts/.

From root: resolves to scripts/Dockerfile.dev (missing).

Ref: proposal.md

## Goals / Non-Goals

**Goals:**
- Script works from repo root or scripts/.

**Non-Goals:**
- Multi-Dockerfile support.

## Decisions

1. **Relative path**:
   - Add `SCRIPT_DIR="$(dirname "$0")"`; `-f "$SCRIPT_DIR/Dockerfile.dev"`.
   - Rationale: Portable, works sourced/exec'd from anywhere.
   - Alternative: `$(cd "$(dirname "$0")"; pwd)/Dockerfile.dev` (absolute).

## Risks / Trade-offs

- [Risk: $0 unset when sourced] → Use `${BASH_SOURCE[0]}` if needed, but exec typical.