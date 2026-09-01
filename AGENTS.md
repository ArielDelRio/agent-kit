# Agent guide

`agent-kit` is a public repository of portable, reusable agent skills.

## Repository conventions

- A skill lives at `skills/<skill-name>/SKILL.md`.
- Use lowercase, hyphenated skill names and concise, discriminating YAML
  frontmatter descriptions.
- Keep each skill self-contained. Place any scripts, references, templates, or
  assets inside the skill only when they directly support its workflow.
- Keep `SKILL.md` as the concise entry point. Put usage guides, templates,
  schemas, examples, and other on-demand documentation in
  `skills/<skill-name>/references/`, and link each file directly from
  `SKILL.md`. Do not create a `docs/` directory or a README by default.
- Use `scripts/` only for executable, repeatable logic and `assets/` only for
  files intended for generated output. Avoid duplicating information between
  `SKILL.md` and `references/`.
- Prefer selective installation. Do not present installation of every skill as
  the default workflow.

## Public-repository boundary

Never add credentials, API tokens, customer data, live MCP configuration, or
organization-only procedures. Configuration files in `configs/` must be safe,
sanitized examples. Each user authenticates with their own account.

## Change workflow

1. Make the smallest focused change that satisfies the request.
2. Update `CHANGELOG.md` for user-visible changes.
3. Verify local skill discovery from the repository root:

   ```bash
   DISABLE_TELEMETRY=1 npx skills add . --list
   ```

4. Follow the contribution and release process in `CONTRIBUTING.md`: pull
   request, owner review, tagged release, then manual user updates.

Skills must remain portable: do not assume a particular agent, provider, or
local account configuration unless the skill's purpose explicitly requires it.
