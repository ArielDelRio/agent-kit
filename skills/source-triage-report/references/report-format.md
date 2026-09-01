# Report format

Use this structure unless the user's requested output requires a compatible
variation. Keep the report self-contained enough for review while linking all
source records for verification.

```markdown
# Source triage report — <date>

## Scope and queries

- Sources requested: <only the user-selected sources>
- Request: <original request>
- Normalized plan: <source, entity, filters, time range, order, limit, detail level>
- Reconnaissance: <what was discovered and criteria derived>
- Collection: <counts, pagination, truncation, unavailable sources>
- Detail handling: <summary, detailed, or full; expanded fields and any condensed material>
- Analysis execution: <subagent or principal-agent fallback>

## Coverage and limitations

<Access gaps, default-cap warning, missing fields, and their effect on confidence.>

## Executive summary

<Counts and the most important review decisions.>

## Candidates for review

| Candidate | Decision | Original priority | Activity and impact | Evidence | Confidence | Suggested human action |
| --- | --- | --- | --- | --- | --- | --- |

For every candidate, identify facts separately from inferences and link its
source record.

## Clusters and relationships

### <Cluster name or representative record>

- Relationship: `recommended cluster` | `cluster candidate` | `possible relation`
- Representative: <ID, title, link, and reason selected>
- Related records: <every ID, title, and link>
- Shared evidence: <facts>
- Inference and confidence: <reason>
- Suggested human action: <non-executed action>

## User reports and operational evidence

| Source | Record | Date | Context | Relationship to candidate | Confidence |
| --- | --- | --- | --- | --- | --- |

Use a context-sufficient excerpt when practical. For large material, give a
faithful concise summary and preserve the link or identifier.

## Exclusions

| Record or group | Reason excluded | Source link |
| --- | --- | --- |

In a `summary` report, group routine exclusions by a meaningful shared reason
and give a count and source query or location. List individual records when
they are related to a candidate or needed to explain a decision. In `detailed`
and `full` reports, include the per-record detail appropriate to the selected
level.

## Sources

<Canonical links or locations consulted.>
```

Use `retain`, `review`, or `discard` only for a review recommendation;
do not use any of them to imply a source mutation. Put original status and
priority beside the recommendation rather than replacing them. If evidence is
contradictory, show both sides and use `review` when it affects the result.
