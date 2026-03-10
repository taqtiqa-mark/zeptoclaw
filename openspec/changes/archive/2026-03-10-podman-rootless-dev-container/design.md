## Context

Dev container rootless Podman (tutorial). Drop docker, use ns keep-id.

## Goals / Non-Goals

**Goals:**
- Perms-free dev (lint/test).

**Non-Goals:**
- Docker support.

## Decisions

1. **Podman-only**:
   - container-base.sh: command -v podman or exit.

2. **Rootless ns** (tutorial):
   - run/build: --userns=keep-id (host uid=cont uid).
   - Volumes /src:rw maps 1:1.

3. **Dockerfile**:
   - apt --no-install-recommends.
   - cargo-nextest=0.10.3.

## Risks / Trade-offs

- Docker users: migrate to Podman.

Comment template:
# Podman rootless tutorial: --userns=keep-id maps host uid/gid to cont (no perms)