## 1. Config Changes

- [x] 1.1 Add `github_token: Option<String>` field to `SkillsConfig` in `src/config/types.rs`
- [x] 1.2 Wire the new config field in `src/config/mod.rs` (add to env overrides if needed)

## 2. Core Implementation

- [x] 2.1 Add `check_skill_md_exists` async function in `src/skills/github_source.rs`
- [x] 2.2 Modify `search_github` function signature to accept `github_token: Option<&str>`
- [x] 2.3 Update `search_github` to use fast/deep mode based on token presence
- [x] 2.4 Implement concurrent SKILL.md checks in deep mode

## 3. Error Handling & Fallbacks

- [x] 3.1 Add graceful error handling for API failures in SKILL.md checks
- [x] 3.2 Implement fallback to fast mode on rate limits or token issues

## 4. Update Callers

- [x] 4.1 Find and update all callers of `search_github` to pass the token from config

## 5. Testing & Validation

- [x] 5.1 Add tests for `check_skill_md_exists` function
- [x] 5.2 Add tests for fast vs deep mode behavior
- [x] 5.3 Verify quality score calculations with/without SKILL.md bonus