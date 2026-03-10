## Why

Fix Trivy vulnerabilities in Dockerfile.dev:
1. DS-0002: Runs as root (add non-root USER).
2. DS-0029: apt without --no-install-recommends (add flag).
3. cargo-nextest unpinned (pin version).

Improves security/scanning score.

## What Changes

- **Dockerfile.dev**: Add `apt-get ... --no-install-recommends`; pin `cargo-nextest =0.10.3`; add `USER rust` (or 1000).
- No breaking changes.

## Capabilities

### New Capabilities
(none)

### Modified Capabilities
(none - Dockerfile hardening; no runtime req changes)

## Impact

- Affected: Dev container builds (lint/test scripts).
- CI: Trivy scan passes.
- Security: Reduced attack surface.