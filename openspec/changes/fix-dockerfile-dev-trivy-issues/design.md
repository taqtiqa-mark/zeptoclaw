## Context

Dockerfile.dev Trivy issues:
1. Root USER.
2. apt no --no-install-recommends.
3. cargo-nextest unpinned.

Ref: proposal.md

## Goals / Non-Goals

**Goals:**
- Pass Trivy dev image scan.

**Non-Goals:**
- Change base image/runtime.

## Decisions

1. **Apt flags**:
   - Add `--no-install-recommends` to reduce bloat.
   
2. **Nextest pin**:
   - `cargo-nextest =0.10.3` (latest stable).

3. **Non-root**:
   - `USER 1000:1000` (standard non-root; Cargo home writable).

## Risks / Trade-offs

- [Nextest pin lags] → Update via renovate.
- [Non-root Cargo] → Volumes mount /src rw.