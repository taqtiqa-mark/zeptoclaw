## Why

Fix path bug in `scripts/lint-container.sh`: `../Dockerfile.dev` fails when invoked from repo root (assumes cwd=scripts/).

Enables running lint script from anywhere.

## What Changes

- **scripts/lint-container.sh:15**: Replace hard-coded `../Dockerfile.dev` with script-relative `"$(dirname \"$0\")/Dockerfile.dev"`.

No breaking changes.

## Capabilities

### New Capabilities
(none)

### Modified Capabilities
(none - script path fix; no spec changes)

## Impact

- Affected: Lint container builds (`scripts/lint-container.sh`).
- Improves: Script portability (run from root/scripts/).