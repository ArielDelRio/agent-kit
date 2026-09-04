# agent-kit

Reusable agent skills maintained by QWave Labs. The kit starts as a personal
repository and is designed to move to the QWave Labs organization later without
changing its installation model.

## Install a skill

Run the interactive installer to choose the skills, target agent, and install
location:

```bash
npx skills add ArielDelRio/agent-kit
```

Or inspect the available skills before selecting one:

```bash
npx skills add ArielDelRio/agent-kit --list
```

Install a specific skill in the current project:

```bash
npx skills add ArielDelRio/agent-kit \
  --skill {skill-name} \
  --agent <your-agent>
```

Use `--global` only when you want a skill available across all of your projects:

```bash
npx skills add ArielDelRio/agent-kit \
  --skill {skill-name} \
  --agent <your-agent> \
  --global
```

Replace `<your-agent>` with the target supported by your local `skills` CLI.
Skills are installed selectively; nothing installs every skill unless you
explicitly choose that option.

## Update a skill

Updates are manual. Read the release notes, then update only the skills you
want:

```bash
npx skills update {skill-name}
```

We publish changes through pull requests, reviewed releases, and this
repository's [changelog](CHANGELOG.md). There are no automatic updates.

## Repository layout

```text
skills/<skill-name>/SKILL.md  # Discoverable, installable skills
configs/                     # Sanitized configuration examples when needed
```

Each skill is self-contained. Add scripts, references, templates, or assets
inside a skill only when they directly support its workflow.

## Available skills

- [`daily-report`](skills/daily-report/SKILL.md): write the HedgeStone daily
  engineering report from the real git, GitHub and Linear record, covering the
  previous working day. Requires explicit invocation. Its
  [worked example](skills/daily-report/references/example-report.md) shows the
  exact header, sections, tables and footer to copy. This skill is
  client-specific rather than portable, and is expected to move to a private
  QWave Labs repository.
- [`linear-ticket-management`](skills/linear-ticket-management/SKILL.md):
  manage Linear projects and tickets with consistent, source-traceable ticket
  creation. Its [usage guide](skills/linear-ticket-management/references/usage.md)
  covers common operations and semantic related-ticket discovery.
- [`source-triage-report`](skills/source-triage-report/SKILL.md): produce a
  read-only, evidence-backed Markdown report from explicitly selected issue,
  feedback, or telemetry sources. Its [report format](skills/source-triage-report/references/report-format.md)
  describes the reviewable output.

## Security

This is a public repository. Never add credentials, API tokens, customer data,
or operational configuration that can access a real service. MCP configuration
files are examples only; every person authenticates with their own account.

## Development and smoke test

The layout can be checked locally before publishing:

```bash
DISABLE_TELEMETRY=1 npx skills add . --list
```

After a change is published, verify remote discovery and an isolated install:

```bash
DISABLE_TELEMETRY=1 npx skills add ArielDelRio/agent-kit --list
```

See [CONTRIBUTING.md](CONTRIBUTING.md) for the full verification checklist.
Repository-wide guidance for coding agents lives in [AGENTS.md](AGENTS.md).
