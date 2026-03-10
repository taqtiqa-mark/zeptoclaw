## Why

Implement Podman rootless dev container per official tutorial: Podman-only + keep-id ns for seamless perms/volumes/builds. Solves docker fallback complexity, root perms, aligns project Podman preference.

## What Changes

- **container-base.sh**: Podman required (drop docker).
- **container_run/build**: --userns=keep-id (UID/GID map).
- **Dockerfile.dev**: Tutorial comments; apt --no-install-recommends; nextest pinned.
- No USER (rootless handles).

**BREAKING**: Docker unsupported.

## Capabilities

### New Capabilities
(none)

### Modified Capabilities
(none - infra hardening)

## Impact

- Dev workflows (lint/test).
- Secure, portable (subuid setup assumed).
- CI: Podman rootless.