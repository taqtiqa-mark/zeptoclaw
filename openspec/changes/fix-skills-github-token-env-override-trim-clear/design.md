## Context

The current env override logic in `src/config/mod.rs` (lines ~1168-1172) sets `self.skills.github_token = Some(val)` where `val` is the raw `os_string.to_str().unwrap()` from `ZEPTOCLAW_SKILLS_GITHUB_TOKEN`. This preserves any leading/trailing whitespace (bad for auth headers) and treats empty string as valid token (cannot clear config.json token).

See proposal.md for motivation.

## Goals / Non-Goals

**Goals:**
- Trim whitespace from the token value before storing.
- If trimmed value is empty, explicitly set `None` to override/clear config.json token.
- Preserve non-empty trimmed tokens as `Some(String)`.

**Non-Goals:**
- Alter handling of other environment variables.
- Change config.json schema or default values.
- Add validation beyond trimming/empty check.

## Decisions

1. **Trimming**: `let trimmed = val.trim().to_string();`
   - Rationale: Simple, standard Rust string handling. `trim()` removes ASCII whitespace.
   - Alternative: `val.trim().to_owned()` - same effect.

2. **Empty handling**: `if trimmed.is_empty() { None } else { Some(trimmed) }`
   - Rationale: Explicit intent to clear on empty env. Matches user request.
   - Alternative: Always `Some` even if empty - rejected, as it prevents clearing.

3. **Placement**: Modify only the `skills.github_token` block to minimize change surface.
   - No broader refactor to keep diff small and focused.

## Risks / Trade-offs

- **[Whitespace in existing tokens]**: Trimmed automatically - improvement, no breakage.
- **[Empty env unintended]**: Rare, but now clears token - documented behavior.
- **No perf impact**: Negligible string ops during config load.

No migration needed: pure code change, backward compatible except intentional clearing.