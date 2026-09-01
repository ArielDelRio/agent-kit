# Linear ticket template

Use this structure for every ticket created with `linear-ticket-management`.
Set the Linear ticket title from **Title**, then use the Markdown block as its
description. Collect the required fields before drafting the ticket. Keep the
**Source** section intact after creation; additions belong in a later context
section.

```markdown
## Context

<What is happening, who is affected, and why it matters.>

## Acceptance criteria (suggested)

- <Observable condition that proves the ticket is complete>

## Source

- Type: <Slack | Gmail | PostHog | Linear | Other>
- Link or ID: <URL, message ID, ticket identifier, or "Not available">
- Provided by: <person or role, when known>
- Summary: <brief account of the originating report or decision>

## References and relationships (suggested)

- Related tickets: <identifiers/URLs, or "None identified">
- Rationale: <why these are related, or "Not applicable">

## Additional context

<Optional implementation-neutral notes added after creation.>
```

## Required Linear fields

| Field | Requirement |
| --- | --- |
| Team | Required; selected explicitly by the user. |
| Project | Required; selected explicitly by the user. |
| Title | Required. |
| Description | Required; use the template above. |
| Source type, link/ID, and summary | Required; link/ID may be `Not available`. |
| Initial state | Required; default to `Backlog` only when available in the selected team. |
| Priority | Required; default to `No priority`. |
| Assignee | Optional; leave it unassigned when the user does not provide one. |
| Acceptance criteria | Suggested; do not block creation when omitted. |
| References and relationship rationale | Suggested; `None identified` is valid. |

## Editing rule

When editing an existing ticket, preserve the original `## Source` block. Append
new material under `## Additional context`, or make a narrowly requested edit
elsewhere after showing the user the proposed change.
