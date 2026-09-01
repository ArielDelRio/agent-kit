# Linear ticket template

Use this structure for every ticket created with `linear-ticket-management`.
Set the Linear ticket title from **Title**, then use the Markdown block as its
description. Collect the required fields before drafting the ticket. Keep the
**Source** section intact after creation; additions belong in a later context
section.

Use the single-source format for one originating record. When two or more
independent reports or decisions substantiate the ticket, use the evidence
table instead. Each row must retain the source type, link or ID, and summary;
include the provider when known. Keep related Linear work in **References and
relationships**, rather than mixing it into the source evidence.

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

### Multiple-source evidence format

Replace the `## Source` section in the template above with this form when it
improves readability:

```markdown
## Source

| Type | Report / ID | Provided by | Summary |
| --- | --- | --- | --- |
| <Slack \| Gmail \| PostHog \| Linear \| Other> | <URL, message ID, ticket identifier, or "Not available"> | <person or role, when known> | <brief account of the originating report or decision> |
| <…> | <…> | <…> | <…> |
```

Keep a source record distinct from a related ticket. A user-provided Slack,
PostHog, or other report belongs in the evidence table; a Linear issue that
defines dependent, prior, or governing work belongs in **References and
relationships**. Add source URLs as native Linear links when creating the
ticket.

## Required Linear fields

| Field | Requirement |
| --- | --- |
| Team | Required; selected explicitly by the user. |
| Project | Required; selected explicitly by the user. |
| Title | Required. |
| Description | Required; use the template above. |
| Source type, link/ID, and summary | Required for every source record; link/ID may be `Not available`. Use the evidence table when multiple sources improve readability. |
| Initial state | Required; default to `Backlog` only when available in the selected team. |
| Priority | Required; default to `No priority`. |
| Assignee | Optional; leave it unassigned when the user does not provide one. |
| Acceptance criteria | Suggested; do not block creation when omitted. |
| References and relationship rationale | Suggested; `None identified` is valid. |

## Editing rule

When editing an existing ticket, preserve the original `## Source` block. Append
new material under `## Additional context`, or make a narrowly requested edit
elsewhere after showing the user the proposed change.
