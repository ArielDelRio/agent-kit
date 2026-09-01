---
name: linear-ticket-management
description: Manage Linear projects and tickets with consistent source-traceable ticket creation, duplicate checks, and user-approved changes.
disable-model-invocation: true
---

# Linear ticket management

Manage projects and tickets through a connected, authenticated Linear MCP. Each
person uses their own Linear access; do not add credentials, workspace IDs, or
live MCP configuration to this skill or its resources.

Use this skill for listing projects, teams, tickets, ticket details, assignments,
ticket creation, edits, state changes, and deliberate ticket discovery. Do not
use it to act on Slack, Gmail, PostHog, or another source system: source links
and context come from the user.

Read [the ticket template](references/ticket-template.md) before creating or
materially editing a ticket. Read [the usage guide](references/usage.md) when
the user asks how the skill works or needs examples.

## Preconditions and scope

- Confirm that a Linear MCP connection is available before an operation. If it
  is not, explain that the user must connect and authenticate their own Linear
  account.
- The user explicitly chooses both the Linear project and team for operations
  that need them. List available projects and teams when necessary; do not
  infer a target from a ticket title or source.
- Resolve available workflow states and eligible users from the selected team
  at run time. Do not assume that every team has the same workflow.
- Read operations need no confirmation. Before every create, edit, assignment,
  state change, or relation write, show the proposed changes and obtain an
  explicit confirmation.

## Core operations

Support these operations directly through the Linear MCP:

- List projects, teams, project tickets, a user's assigned tickets, and users.
- Retrieve a ticket with its details and existing relations.
- Create a ticket from the required template fields.
- Edit supplied ticket fields, assign or unassign a user, and update state.
- Add a native Linear relation only after the user chooses it.

For a new ticket, use `Backlog` only when that exact state is available for the
selected team. Otherwise, present the available states and ask the user to
choose. Default priority to **No priority** unless the user supplies one.

## Creation and duplicate check

Collect every required template field before proposing the ticket. Preserve the
original source record in the description; later edits may append context but
must not overwrite it. When the source includes a URL, also add it as a native
Linear link alongside the source record.

Before proposing creation, automatically search the selected project and team
for plausible duplicates. Compare title, problem, expected outcome, source
context, and relevant labels or components when present. This is a focused
duplicate check, not the explicit related-ticket discovery mode below.

- If no plausible duplicate is found, continue without mentioning the search.
- If candidates are found, show only the meaningful candidates with the reason
  they may be duplicates. The user decides whether to create a new ticket,
  update an existing one, or mark/link a duplicate. Never decide this alone.

## Explicit related-ticket discovery

Run this only when the user explicitly asks to find related tickets or provides
context for that purpose. Default the scope to the selected project and team;
the user may explicitly expand it to the workspace.

When the host supports subagents, delegate the discovery to one subagent. Give
it the user-provided context, selected scope, and a read-only task. It should
search using multiple conceptually equivalent queries, inspect the most
promising ticket details, and return at most ten candidates ordered by
confidence. Do not make writes in this mode. If the host cannot delegate a
subagent, say that this discovery mode requires subagent support rather than
silently claiming it was delegated.

For each candidate, return its identifier, title, URL, classification, a
confidence score for ordering, and a brief evidence-based reason:

- **Possible duplicate**: substantially the same problem and expected outcome.
- **Strongly related**: different tickets sharing a cause, component, workflow,
  or dependency.
- **Weakly related**: overlapping domain or context without a clear dependency.

Do not surface superficial keyword matches as candidates. It is valid to report
that no related ticket was found. The user, not the agent, decides whether a
relation should be created and which native Linear relation is appropriate.

## Closing a ticket

When the user asks to move a ticket to a completed or canceled state, first
show the proposed state change. After it succeeds, use the preserved source
record to offer a concise, textual follow-up recommendation (for example,
whether the reporter may need an update). Do not send a message or act in the
external source system unless the user separately directs that work.
