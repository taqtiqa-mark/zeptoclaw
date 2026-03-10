## Context

docs/scripts.md stale vs current Podman rootless.

## Goals / Non-Goals

**Goals:**
- Accurate setup/usage post-changes.

**Non-Goals:**
- New sections.

## Decisions

1. **Structure**:
   - Setup: Podman req, volumes, sccache.
   - Usage: ./scripts/* with rootless notes.
   - Prune/Troubleshoot: podman volume prune.

2. **Comments**:
   - "# Rootless: --userns=keep-id perms".

## Risks

- Stale if scripts change (renovate docs?).