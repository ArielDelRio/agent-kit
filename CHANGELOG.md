# Changelog

All notable changes to this kit are recorded here.

## Unreleased

### Added

- `daily-report`, a skill for writing the HedgeStone daily engineering report
  from the git, GitHub and Linear record, with a Monday range rule for the
  weekend, a mandatory honest-accounting section, and a worked example in
  `references/`. It requires explicit user invocation, and carries a maintainer
  note recording that its client-specific references should be removed or
  parameterized, pending a move to a private QWave Labs repository.
- `source-triage-report`, a read-only, multi-source triage skill that produces
  evidence-backed Markdown reports without modifying the source systems.
- Agent-neutral repository guidance in `AGENTS.md` and a `CLAUDE.md` pointer.
- `linear-ticket-management`, a portable Linear MCP skill with a consistent
  ticket template, duplicate checks, explicit related-ticket discovery, and
  onboarding documentation.
- Project-local installation of the upstream `skill-creator` skill for Codex.
- Project-local installation of the upstream `grill-me` skill.
- Project-local installations of the upstream `grill-with-docs`, `grilling`,
  and `handoff` skills, with Claude entry points.

### Removed

- `weekly-reporting`, which existed only as a scaffold test. Its references
  were dropped from `README.md` and `CONTRIBUTING.md`.

### Changed

- Write default `source-triage-report` Markdown output to the user's OS
  temporary directory rather than the current workspace.
- Improved the Linear ticket template's multi-source evidence format with a
  traceable table and clear separation between source reports and related
  Linear work.
- Refocused `source-triage-report` on requested ticket findings and removed
  routine query-plan, collection, and limitations sections from its output.
- Added `summary` (default), `detailed`, and `full` detail levels to
  `source-triage-report`, including bounded, selective collection guidance.
- Require explicit user invocation for `source-triage-report` and prevent its
  analysis subagent from making source calls.
- Clarified that Linear MCP access is required for both reads and writes, while
  explicit user confirmation is required only before writes.
- Generalized installation and smoke-test documentation to avoid preferring a
  particular coding agent.
- Documented the standard skill layout: concise `SKILL.md` entry points with
  on-demand documentation in `references/`.
- Required explicit invocation for `linear-ticket-management`.
- Documented interactive and direct installation for
  `linear-ticket-management`.
- Made acceptance criteria and ticket relationship context optional suggestions
  for Linear ticket creation.
- Made the Linear ticket assignee optional.

## 0.1.0 - 2026-08-31

### Added

- Initial public `agent-kit` scaffold.
- `weekly-reporting`, a portable skill for drafting weekly status reports.
