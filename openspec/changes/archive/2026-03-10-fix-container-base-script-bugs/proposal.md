## Why

Fix two bugs in `scripts/container-base.sh`:
1. Tilde `~` does not expand inside double quotes, causing sccache mount to use literal `~` instead of `$HOME`.
2. `container_run "$@"` executes unconditionally even when the script is sourced by other scripts.

These fixes ensure correct caching and safe sourcing.

## What Changes

- **scripts/container-base.sh:34**: Replace `~` with `$HOME` in the sccache volume mount argument.
- **scripts/container-base.sh:46**: Add `if [[ "${BASH_SOURCE[0]}" == "${0}" ]]; then container_run "$@"; fi` guard to run only when executed directly.

No breaking changes.

## Capabilities

### New Capabilities
(none)

### Modified Capabilities
(none - infrastructure script fixes; no changes to agent capabilities or requirements)

## Impact

- Affected: Docker container builds using `container-base.sh`.
- Dependencies: sccache caching reliability.
- Systems: CI/CD pipelines invoking container builds.