## Why

The GitHub skill search currently computes quality scores based on repo metadata (stars, license, description) but doesn't check for the presence of `SKILL.md`, which indicates a properly documented skill. This leads to lower-quality search results, as repos without documentation rank equally to those with it.

Adding SKILL.md presence detection would improve search accuracy by boosting repos that follow the skill documentation standard.

## What Changes

- Enhance the GitHub skill search to optionally check for `SKILL.md` existence in repo roots
- Add `github_token` config field to enable authenticated API calls (higher rate limits)
- Implement fast mode (metadata-only) vs deep mode (with SKILL.md checks) based on token availability
- Update quality scoring to include +0.3 bonus for repos with SKILL.md

## Capabilities

### New Capabilities
- `skill-search-quality`: Improved accuracy by detecting documented skills

### Modified Capabilities
- `github-skill-search`: Enhanced with optional deep scanning

## Impact

- `src/skills/github_source.rs`: Add SKILL.md checking logic and token parameter
- `src/config/types.rs`: Add `github_token` field to `SkillsConfig`
- `src/config/mod.rs`: Wire the new config field