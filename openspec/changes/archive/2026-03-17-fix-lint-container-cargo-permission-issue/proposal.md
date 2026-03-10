## Why

The `lint-container.sh` script fails with permission denied errors when Cargo attempts to write to its cache and index directories (/usr/local/cargo/registry/) inside the container. This prevents updating the crates.io index and downloading crates like `aead-0.5.2`, breaking the linting process in containerized environments.

## What Changes

- Modify `scripts/lint-container.sh` to run Cargo as a non-root user, by adjusting UID/GID when running the container.
- Ensure proper ownership or permissions for Cargo's registry directories within the container.
- Add volume mounts if necessary to persist or share the cargo registry outside the container to avoid permission issues.
- No breaking changes.

## Capabilities

### New Capabilities

### Modified Capabilities

## Impact

- Affected: `scripts/lint-container.sh`
- Dependencies: Docker/container runtime, Cargo
- Systems: Local development, CI/CD pipelines using containerized linting
