# Changelog

All notable changes to this kit are recorded here.

## Unreleased

### Added

- `source-triage-report`, a read-only, multi-source triage skill that produces
  evidence-backed Markdown reports without modifying the source systems.
- Agent-neutral repository guidance in `AGENTS.md` and a `CLAUDE.md` pointer.
- `linear-ticket-management`, a portable Linear MCP skill with a consistent
  ticket template, duplicate checks, explicit related-ticket discovery, and
  onboarding documentation.
- Project-local installation of the upstream `skill-creator` skill for Codex.
- Project-local installation of the upstream `grill-me` skill.

### Changed

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
