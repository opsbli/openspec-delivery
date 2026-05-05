## ADDED Requirements

### Requirement: Frontend skill selection

`SKILL.md` SHALL define a minimal selection order for frontend work that distinguishes design judgment from implementation.

#### Scenario: New UI direction

- **WHEN** a frontend task needs a new visual direction, layout strategy, or interaction pattern
- **THEN** the agent SHALL use `ui-ux-pro-max` before implementation work
- **AND** the agent SHALL use `frontend-design` for the concrete frontend design/implementation step

#### Scenario: Existing design constraints

- **WHEN** a frontend task already has a clear design system or established UI constraints
- **THEN** the agent MAY go directly to `frontend-design`
- **AND** the agent SHALL still use `web-design-engineer` as the workflow entrypoint for frontend work

#### Scenario: Frontend rule entry

- **WHEN** `AGENTS.md` or `CLAUDE.md` describes frontend work
- **THEN** the entry text SHALL mention `ui-ux-pro-max` and `frontend-design`
- **AND** the entry text SHALL remain concise without duplicating full skill documentation
