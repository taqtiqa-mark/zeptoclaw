## Why

Build fails: Malformed `--mount=` inside shell (sh: not found); podman legacy no frontend.

## What Changes

- Syntax: `--mount=` after `RUN` in Dockerfile.dev.
- Fallback: Comment `--mount=` (# BuildKit).
- Script: Revert invalid `--buildkit`.
- Docs: Podman conf, sccache.
- Polish: `.cargo/config.toml` registry; auto-volumes.
- sccache: Auto-opt in scripts.

## Capabilities

none

## Impact

PR builds unblocked (podman/docker); fast runtime (volumes/sccache).