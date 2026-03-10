## 1. Fix apt

- [ ] 1.1 Edit Dockerfile.dev apt RUN: add `--no-install-recommends`.

## 2. Pin nextest

- [ ] 2.1 Edit cargo-nextest RUN: `--locked cargo-nextest=0.10.3`.

## 3. Add non-root USER

- [ ] 3.1 Add `USER 1000` before WORKDIR.

## 4. Verify

- [ ] 4.1 Run trivy image --input tar:dockerfile-dev.tar (0 issues).
- [ ] 4.2 Build/test container_run (no rw issues).