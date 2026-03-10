## 1. Update Container Base Script

- [x] 1.1 Modify the REGISTRY_MOUNT variable in `scripts/container-base.sh` to mount the volume at `/cargo-home:rw,U` instead of `/src/Cargo-registry:rw,U`
- [x] 1.2 Add `-e CARGO_HOME=/cargo-home` to the `podman run` command in the `container_run` function
- [x] 1.3 Ensure the volume `zeptoclaw-registry_cache` is created (already handled by existing printf loop)

## 2. Testing and Verification

- [x] 2.1 Run `./scripts/lint-container.sh` and confirm no permission denied errors occur during Cargo index update and crate downloads. Mount /clippy.toml if needed (e.g., add `-v $(pwd)/scripts/artifacts/clippy.toml:/clippy.toml:Z` to container_run).
- [x] 2.2 Verify that the Cargo registry persists across multiple runs (e.g., check if crates are not re-downloaded)
- [x] 2.3 Test with a clean volume if possible (podman volume rm zeptoclaw-registry_cache && recreate) to ensure initial setup works

## 3. Documentation

- [x] 3.1 Update any relevant documentation or comments in `scripts/container-base.sh` explaining the CARGO_HOME override for rootless Podman
