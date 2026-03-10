## 1. Fix Dockerfile path

- [x] 1.1 Edit `scripts/lint-container.sh`: Add `SCRIPT_DIR="$(dirname "$0")"` after source.
- [x] 1.2 Edit line 15: Replace `-f ../Dockerfile.dev` with `-f "$SCRIPT_DIR/Dockerfile.dev"`.

## 2. Verify

- [x] 2.1 From repo root: `scripts/lint-container.sh` builds image successfully.
- [x] 2.2 From scripts/: `./lint-container.sh` (no regressions).
