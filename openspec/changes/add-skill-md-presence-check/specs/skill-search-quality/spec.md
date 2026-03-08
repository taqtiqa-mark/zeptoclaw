## ADDED Requirements

### Requirement: Skill Search Quality Improvement

The system SHALL prioritize skills with proper documentation in search results.

#### Scenario: Documented skills rank higher

- **WHEN** searching for skills on GitHub
- **THEN** repositories containing `SKILL.md` in the root SHALL receive a quality score bonus of +0.3
- **AND** the bonus SHALL only apply when deep scanning is enabled