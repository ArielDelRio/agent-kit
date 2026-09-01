# Contributing

## Adding or changing a skill

1. Put each skill at `skills/<skill-name>/SKILL.md`.
2. Use a lowercase, hyphenated `name` and a concise, discriminating
   `description` in YAML frontmatter.
3. Keep the skill self-contained. Add scripts, references, or assets only when
   they directly support that skill.
4. Do not include secrets, customer data, or live service configuration.
5. Open a pull request. Material changes require review before release.

## Verify before release

From the repository root, ensure local discovery works:

```bash
DISABLE_TELEMETRY=1 npx skills add . --list
```

After publishing the branch or release, verify the public repository can be
discovered:

```bash
DISABLE_TELEMETRY=1 npx skills add ArielDelRio/agent-kit --list
```

Then install a selected skill in a disposable directory. This must not modify a
real project or global agent configuration:

```bash
TASK_TEST_DIR="$(mktemp -d)"
cd "$TASK_TEST_DIR"
DISABLE_TELEMETRY=1 npx skills add ArielDelRio/agent-kit \
  --skill weekly-reporting --agent codex --copy --yes
test -f .agents/skills/weekly-reporting/SKILL.md
npx skills list --agent codex --json
```

Record user-visible changes in [CHANGELOG.md](CHANGELOG.md). Release only after
the smoke test passes.

