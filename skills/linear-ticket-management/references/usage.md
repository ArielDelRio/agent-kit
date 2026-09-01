# Using `linear-ticket-management`

## Prerequisite

Connect an authenticated Linear MCP using your own account. The skill does not
include credentials, workspace configuration, or access to external source
systems.

## Common requests

Ask in plain language or invoke the skill explicitly. Start by selecting a
project and team whenever the request needs a target.

```text
List the Linear projects I can access.
List tickets in project QWave Second Brain and team QWave Labs.
Show the details and relationships for QWA-123.
Show tickets assigned to me in the QWave Labs team.
Assign QWA-123 to Ana and move it to In Progress.
```

For creation, provide the source and enough information for the ticket
template. The skill asks for any missing required information, runs a focused
duplicate check, and shows the final draft before creating anything.

```text
Create a ticket in project QWave Second Brain and team QWave Labs.
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
Find tickets related to “document events and user deliveries” in project QWave
Second Brain and team QWave Labs.
```

The skill delegates a read-only search to a subagent when the host supports
subagents. It returns no more than ten candidates, classified as possible
duplicate, strongly related, or weakly related, with reasoning. It never creates
or changes relations until you choose what to do.

## Closing and follow-up

```text
Move QWA-123 to Done.
```

After the confirmed state change, the skill may recommend a textual follow-up
based on the ticket's recorded source. It does not contact Slack, Gmail,
PostHog, or another external service on its own.
