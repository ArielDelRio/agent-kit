# Using `linear-ticket-management`

## Install

After this skill is merged, install it from the repository in either mode:

```bash
# Interactive selector: choose linear-ticket-management and your agent.
npx skills add ArielDelRio/agent-kit

# Direct installation: install only this skill.
npx skills add ArielDelRio/agent-kit --skill linear-ticket-management
```

The interactive selector is intended for a normal terminal. When run inside
Codex, the CLI detects the host and installs non-interactively; use the direct
command to select this skill there.

## Prerequisite

This skill requires a connected, authenticated Linear MCP using your own
account. Without it, the skill cannot list, read, create, or update Linear
tickets. It does not include credentials, workspace configuration, or access to
external source systems.

## Invoke the skill

Explicit invocation is required; the skill is not selected automatically.

| Agent | Invocation |
| --- | --- |
| Codex | `$linear-ticket-management <request>` |
| Claude Code | `/linear-ticket-management <request>` |

For another agent, use its documented explicit skill-invocation syntax. Start
by selecting a project and team whenever the request needs a target.

## Common requests

```text
$linear-ticket-management List the Linear projects I can access.
$linear-ticket-management List tickets in project QWave Second Brain and team QWave Labs.
$linear-ticket-management Show the details and relationships for QWA-123.
$linear-ticket-management Show tickets assigned to me in the QWave Labs team.
$linear-ticket-management Assign QWA-123 to Ana and move it to In Progress.
```

For creation, provide the source and enough information for the ticket
template. The skill asks for any missing required information, runs a focused
duplicate check, and shows the final draft before creating anything.

```text
$linear-ticket-management Create a ticket in project QWave Second Brain and team QWave Labs.
Source: Slack, https://example.com/message/123.
The onboarding flow does not explain when the vault setup has finished.
Assign it to me. The acceptance criterion is that users see a completion state
with the vault location. Keep the priority as No priority and start it in
Backlog.
```

## Finding related tickets

This is separate from the automatic duplicate check used during ticket creation.
Ask explicitly when you want semantic discovery:

```text
$linear-ticket-management Find tickets related to "document events and user deliveries" in project QWave Second Brain and team QWave Labs.
```

The skill delegates a read-only search to a subagent when the host supports
subagents. It returns no more than ten candidates, classified as possible
duplicate, strongly related, or weakly related, with reasoning. It never creates
or changes relations until you choose what to do.

## Closing and follow-up

```text
$linear-ticket-management Move QWA-123 to Done.
```

After the confirmed state change, the skill may recommend a textual follow-up
based on the ticket's recorded source. It does not contact Slack, Gmail,
PostHog, or another external service on its own.
