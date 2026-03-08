## Context

The GitHub skill search (`search_github`) currently performs a single API call to search repositories and computes quality scores based on metadata. We want to optionally enhance this with SKILL.md presence checks to improve result quality.

## Goals / Non-Goals

**Goals:**
- Add SKILL.md presence detection to boost quality scores
- Support fast mode (no token) and deep mode (with token)
- Maintain backward compatibility and performance

**Non-Goals:**
- Change the search query or result format
- Add SKILL.md content analysis (just presence)
- Support tokens for other GitHub APIs

## Decisions

### Decision 1: Config Placement

Add `github_token: Option<String>` to `SkillsConfig` for clean isolation from marketplace settings.

### Decision 2: API Checking Strategy

Use GitHub's `GET /repos/{owner}/{repo}/contents/SKILL.md` endpoint. 200 = exists, 404 = doesn't. Handle other errors gracefully (e.g., private repos as no bonus).

### Decision 3: Performance Optimization

In deep mode, check SKILL.md for all repos concurrently (up to 20 parallel requests) to minimize latency. Fall back to fast mode if rate limited.

### Decision 4: Error Handling

- Network failures: Score as `has_skill_md = false`
- Rate limits: Log warning and switch to fast mode
- Invalid tokens: Fall back to fast mode

### Decision 5: Function Signature

Extend `search_github(client, query, topics)` to `search_github(client, query, topics, github_token: Option<&str>)` and update all callers to pass the token from config.

### Decision 6: No Caching

Do not cache SKILL.md checks—repos can add/remove files, and cache invalidation adds complexity without clear benefit for this use case.