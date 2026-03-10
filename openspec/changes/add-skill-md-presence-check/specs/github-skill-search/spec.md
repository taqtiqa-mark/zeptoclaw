## MODIFIED Requirements

### Requirement: GitHub Skill Search Enhancement

The GitHub skill search SHALL support optional deep scanning for documentation presence.

#### Scenario: Fast mode without token

- **WHEN** no GitHub token is configured
- **THEN** search SHALL use fast mode with 1 API call
- **AND** quality scores SHALL be computed without SKILL.md bonus

#### Scenario: Deep mode with token

- **WHEN** a GitHub token is configured
- **THEN** search SHALL use deep mode with additional API calls to check SKILL.md
- **AND** quality scores SHALL include SKILL.md presence bonus