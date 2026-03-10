## 1. Implement Code Change

- [x] 1.1 Locate the block in `src/config/mod.rs` at lines 1168-1170:
```
        if let Ok(val) = std::env::var(\"ZEPTOCLAW_SKILLS_GITHUB_TOKEN\") {
                self.skills.github_token = Some(val);
```
- [x] 1.2 Replace `self.skills.github_token = Some(val);` with:
```
                let trimmed = val.trim();
                self.skills.github_token = if trimmed.is_empty() {
                    None
                } else {
                    Some(trimmed.to_string())
                };
```
  (Preserve exact indentation.)

## 2. Run Quality Gates

- [ ] 2.1 `cargo fmt -- --check`
- [ ] 2.2 `cargo clippy -- -D warnings`
- [ ] 2.3 `cargo nextest run --lib`
- [ ] 2.4 `cargo test --doc`

## 3. Verification

- [ ] 3.1 Confirm no regressions in config loading (manual or add unit test in `src/config/mod.rs`)
- [ ] 3.2 Test env override: set `ZEPTOCLAW_SKILLS_GITHUB_TOKEN=\"  test \"`, check trimmed; set `\"\"`, check None.

## 4. Project Protocol

- [ ] 4.1 Check/create GitHub issue (`bug`, `area:config`, `P2`)
- [ ] 4.2 Create PR with `Closes #N`, wait for CI/user approval