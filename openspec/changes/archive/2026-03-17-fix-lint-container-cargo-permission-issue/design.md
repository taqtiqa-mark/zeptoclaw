## Context

The `lint-container.sh` script uses Podman rootless containers to run Cargo linting commands (clippy, fmt, doc tests). The container runs with `--userns=keep-id`, executing as the host's non-root user. The base image (rust:1.93-slim) sets `CARGO_HOME=/usr/local/cargo`, but this directory is owned by root with 755 permissions, preventing the non-root user from writing to it for registry index caching and crate downloads.

## Goals / Non-Goals

**Goals:**
- Enable Cargo to write to its registry cache and index without permission errors in rootless Podman.
- Maintain persistence of the Cargo registry across container runs using volumes.
- Keep the fix minimal, affecting only the container runtime setup.

**Non-Goals:**
- Changing the container image or adding root privileges.
- Optimizing for Docker (focus on Podman as primary runtime).
- Handling other Cargo-related permissions (e.g., git cache) unless they arise.

## Decisions

- **Set CARGO_HOME to a dedicated writable volume directory:** Override the image's `CARGO_HOME=/usr/local/cargo` by passing `-e CARGO_HOME=/cargo-home` to the container run command. This allows Cargo to use a user-writable path.
  
  Rationale: Direct chown of /usr/local/cargo is not feasible in rootless mode (confirmed root-owned in base and custom images, e.g., registry/ 755 root:root). Moving to a volume-mounted directory ensures writability and persistence. Alternatives considered: 
  - Using `/tmp/cargo` (non-persistent, loses cache on each run).
  - Mounting directly over `/usr/local/cargo` (risks overwriting image files like bin/, complicated partial mounts).
  - Baking ENV CARGO_HOME=/cargo-home in Dockerfile.dev (requires rebuilds; runtime override more flexible).

- **Repurpose existing volume for Cargo home:** Use the pre-existing `zeptoclaw-registry_cache` volume, mounted as `-v zeptoclaw-registry_cache:/cargo-home:rw,U`. Create the volume if missing (already handled in container-base.sh).

  Rationale: Avoids introducing new volumes. The volume name suggests its intended use for registry caching. If needed, it can be renamed later.

- **Update container_run function:** Modify `scripts/container-base.sh` to include the `-e CARGO_HOME=/cargo-home` environment variable and adjust the REGISTRY_MOUNT path to `/cargo-home`.

  Rationale: Centralizes the change in the shared base script used by dev tools.

## Risks / Trade-offs

- [Volume growth:] Cargo registry can grow large (GBs) over time with many dependencies. → Monitor volume size; add cleanup script if needed. Users can prune with `podman volume prune`.
- [Compatibility with existing cache:] If users have existing cache in default locations, it won't be used initially. → First run will re-download crates, but subsequent runs persist.
- [Podman-specific:] Fix assumes Podman; Docker runs might still have issues if switched. → Document that Podman is preferred for rootless.

## Migration Plan

1. Update `container-base.sh` with the new env and mount path.
2. Existing volumes remain compatible (content will be used under new path).
3. No rollback needed; revert script changes if issues.
4. Test by running `./scripts/lint-container.sh` and verifying no permission errors.

## Open Questions

- Confirmed: Dockerfile.dev does not override CARGO_HOME; it defaults to /usr/local/cargo, which is root-owned (e.g., registry/ dir 755 root:root). Runtime -e CARGO_HOME=/cargo-home is essential for non-root writes.
- Does Cargo need additional env vars (e.g., for git config) in rootless mode?
- For Podman 5.7 builds, use buildah with --userns=host to bypass userns mapping issues (crun gid_map write denied); upgrade to 5.8+ for native keep-id support.
