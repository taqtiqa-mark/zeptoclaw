## 1. Fix sccache mount path bug

- [x] 1.1 Edit `scripts/container-base.sh` at line 34: replace `~` with `$HOME` in the `-v` docker volume mount argument for sccache.

## 2. Add execution guard for container_run

- [x] 2.1 Edit `scripts/container-base.sh` around line 46: wrap the `container_run \"$@\"` invocation with:
  ```
  if [[ \"${BASH_SOURCE[0]}\" == \"${0}\" ]]; then
      container_run \"\$@\"
  fi
  ```

## 3. Verify fixes

- [x] 3.1 Test tilde fix: run docker command manually, confirm sccache volume mounts to actual $HOME path.
- [x] 3.2 Test guard: source the script in another bash script, confirm `container_run` does not execute; run directly, confirm it does.
- [x] 3.3 Run full container build (e.g., docker build) to ensure no regressions.