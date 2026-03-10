## Why

The environment override for `ZEPTOCLAW_SKILLS_GITHUB_TOKEN` currently stores the raw value without trimming, which can introduce leading/trailing whitespace into Authorization headers. Additionally, it cannot explicitly clear an existing token from `config.json` by setting an empty environment variable.

## What Changes

- Update the logic around `self.skills.github_token`:
  - Trim the environment value (`let trimmed = val.trim()`).
  - If `trimmed.is_empty()`, set `self.skills.github_token = None`.
  - Otherwise, set `self.skills.github_token = Some(trimmed.to_string())`.

## Capabilities

### New Capabilities

(None - this is a targeted bug fix.)

### Modified Capabilities

(None - no spec-level requirement changes; purely implementation fix in config parsing.)

## Impact

- Primary: `src/config/mod.rs` (lines ~1168-1172, env override block for skills.github_token).
- Affects: Skills GitHub token usage in Authorization headers (improved robustness).