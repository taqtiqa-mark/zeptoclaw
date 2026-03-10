## ADDED Requirements

### Requirement: Containerized devtools scripts exist and are executable
The project SHALL provide bash scripts in `scripts/` for containerized test, lint, and bench runs.

#### Scenario: Scripts are present and executable
- **WHEN** listing `scripts/`
- **THEN** container-base.sh, test-container.sh, lint-container.sh, bench-container.sh exist and have +x permissions.

### Requirement: Scripts support Docker and Podman
The scripts SHALL detect $CONTAINER_RUNTIME or default to 'docker', and use appropriate CLI for run/rm/volume.

#### Scenario: Docker runtime
- **WHEN** $CONTAINER_RUNTIME=docker or unset, run `./scripts/test-container.sh`
- **THEN** uses `docker run --rm ...`

#### Scenario: Podman runtime
- **WHEN** $CONTAINER_RUNTIME=podman, run `./scripts/test-container.sh`
- **THEN** uses `podman run --rm ...`

### Requirement: Scripts use consistent Rust image and mounts
The scripts SHALL use `rust:1.80-slim`, mount pwd:/src:rw, and cache mounts/volumes for /src/target, /src/Cargo-registry, /src/benches/target.

#### Scenario: Standard mounts
- **WHEN** run script
- **THEN** container has /src as pwd, rust toolchain, cache for target/registry.

### Requirement: test-container.sh runs cargo tests
The script SHALL support [lib|integration|all], run nextest --lib or test, passthru args.

#### Scenario: Lib tests
- **WHEN** `./scripts/test-container.sh lib`
- **THEN** runs `cargo nextest run --lib`, outputs results.

### Requirement: lint-container.sh runs lints
The script SHALL run clippy all-targets -D warnings, fmt --all --check, test --doc, fail-fast.

#### Scenario: Full lint
- **WHEN** `./scripts/lint-container.sh`
- **THEN** runs all linters sequentially, exits non-zero on failure.

### Requirement: bench-container.sh runs benches
The script SHALL run `cargo bench message_bus --no-run`.

#### Scenario: Bench no-run
- **WHEN** `./scripts/bench-container.sh`
- **THEN** prints bench summary without executing.

### Requirement: Caching speeds up runs without integrity compromise
Cache SHALL persist target/registry between runs, but tests SHALL use fresh temp files/DB.

#### Scenario: Cache hit speeds test
- **WHEN** run test-container.sh twice
- **THEN** second run uses cached deps/target, faster, same results.

### Requirement: Optional sccache compile caching
The scripts SHALL support `--sccache` flag to enable sccache daemon with dedicated cache volume.

#### Scenario: sccache enabled
- **WHEN** `./scripts/test-container.sh lib --sccache`
- **THEN** installs sccache, sets `RUSTC_WRAPPER=sccache`, mounts cache volume, uses compile cache on hit.