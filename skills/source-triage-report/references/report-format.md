# Report format

Use a findings-first structure. Keep the report self-contained enough for
review while linking source records for verification. Do not include `Scope and
queries`, `Coverage and limitations`, reconnaissance, pagination, collection
counts, or agent-execution details as routine sections. Those are working
notes, not the result the user asked to read.

For a request for identified tickets, use the ticket-focused format below. For
a broad triage, retain only the sections that help a person decide what needs
attention; omit empty sections.

```markdown
# Source triage report — <date>

## Executive summary

<The direct answer: what is happening, why it matters, and the current
decision or next human action. For a single ticket, this can be a short
paragraph rather than a count.>

## Tickets

### <Ticket ID — title>

| Field | Current information |
| --- | --- |
| Status | <status> |
| Priority / owner | <priority and owner, if available> |
| Impact | <reported or observed impact> |
| Affected area | <route, product, component, or user context> |

**Reported problem:** <concise, context-sufficient customer report or summary>

**What the evidence shows:** <observed facts, followed by a clearly labelled
inference only when useful.>

**What to do next:** <the concrete human decision, investigation, or response
that the evidence supports.>

## Related issues and evidence

### <Related issue or linked investigation>

- Relationship: `standalone` | `recommended cluster` | `cluster candidate` |
  `possible relation`
- Relevant evidence: <facts that change the requested ticket's interpretation>
- Conclusion and confidence: <reason>

## Sources

<Canonical links or locations consulted.>
```

Use a table for multiple tickets when it makes comparison easier; preserve the
reported problem, impact, evidence, and next action for each ticket. Do not
label an explicitly requested ticket `discard` merely because it is not a
priority candidate.

Include related issues and supporting evidence only when they materially change
the ticket's interpretation. Do not surface broadly adjacent issues just
because they were encountered during collection.

If a limitation changes the conclusion—for example, a missing ticket thread,
truncated result set, or unavailable source—state it briefly beside the
affected ticket or evidence. Do not add a standalone limitations section for
ordinary boundaries of the request.

Use `retain`, `review`, or `discard` only for a review recommendation; do not
use any of them to imply a source mutation. Put original status and priority
beside the recommendation rather than replacing them. If evidence is
contradictory, show both sides and use `review` when it affects the result.
