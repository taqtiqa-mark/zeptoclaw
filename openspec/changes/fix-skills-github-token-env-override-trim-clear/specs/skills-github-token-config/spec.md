## ADDED Requirements

### Requirement: Trim and clear logic for skills GitHub token env override
The configuration loader SHALL process the `ZEPTOCLAW_SKILLS_GITHUB_TOKEN` environment variable as follows:
- Trim leading and trailing whitespace from the value.
- If the trimmed value is empty, set `skills.github_token` to `None` (overriding any config.json value).
- Otherwise, set `skills.github_token` to `Some(trimmed_value)`.

#### Scenario: Token with leading/trailing whitespace
- **WHEN** `ZEPTOCLAW_SKILLS_GITHUB_TOKEN="  ghp_abc123def  "`
- **THEN** `skills.github_token = Some("ghp_abc123def")`

#### Scenario: Empty environment variable
- **WHEN** `ZEPTOCLAW_SKILLS_GITHUB_TOKEN=""` (or equivalent empty after trim)
- **THEN** `skills.github_token = None`

#### Scenario: Whitespace-only environment variable
- **WHEN** `ZEPTOCLAW_SKILLS_GITHUB_TOKEN="   "`
- **THEN** `skills.github_token = None`

#### Scenario: Normal token without whitespace
- **WHEN** `ZEPTOCLAW_SKILLS_GITHUB_TOKEN="ghp_xyz789"`
- **THEN** `skills.github_token = Some("ghp_xyz789")`