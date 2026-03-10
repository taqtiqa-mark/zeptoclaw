# No Specification Changes

This change fixes bugs in `scripts/container-base.sh`:
- Tilde expansion in sccache mount.
- Execution guard for `container_run`.

No new or modified capabilities or requirements. Implementation details only (see design.md).