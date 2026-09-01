# Source guidance

## Read scope

The user chooses the source or sources. Do not broaden that set, even if a
different connected source might supply helpful context. A source may be a
known service, a connected custom tool, or user-supplied local data that the
agent can read. It is not a request to configure an integration or to obtain
access.

For a source not already understood in the current run, first make the
smallest useful read-only discovery request. Learn the fields and values that
actually exist rather than assuming common names such as `status`, `priority`,
or `updated_at`.

Capture, where available:

- Stable identifier and canonical URL or source location.
- Title, concise description, type, project/team/component, and labels.
- State/status and its terminal or active meaning.
- Priority/impact values and their source-defined meaning.
- Creation, occurrence, resolution, and meaningful activity timestamps.
- Ownership, current-work indicators, dependencies, and blocking signals.
- Feedback, reports, affected users, event/error volume, and their links.

Do not treat any field as an activity signal solely by name. For example, a
recent `updatedAt` can result from a bulk update. Explain the evidence used for
activity in the report.

## Query planning

Normalize the user request before querying. State the source, entity type,
filters, ordering, time window, requested limit, and detail level. The detail
level is **summary** by default; accept **detailed** or **full** when the user
requests them. A request such as
“everything” remains bounded by the default cap of 500 records per source when
no limit is supplied; it is not evidence that the source has been exhaustively
read. Record that limitation.

Use the source's supported server-side filters and pagination when possible.
Prefer field selection and compact list queries for the first pass. Fetch
expanded record detail only according to the selected detail level, in bounded
batches where possible. A `full` report is comprehensive for the collected
scope, but is not permission to load arbitrary raw payloads, unbounded event
streams, or repetitive nested content that does not improve review; record
counts, representative evidence, and links instead.
If a source cannot filter as requested, collect only a reasonable candidate
set, apply a transparent local filter, and describe that limitation. Do not
claim a complete result set when pagination, access, or query support prevents
one.

## Interpreting activity and importance

Derive source-specific signals after reconnaissance. Typical evidence can
include non-terminal workflow states, work in progress, recent unresolved
occurrences, an active owner, user impact, or a priority value. These are
examples, not universal mappings.

The default triage favors records with a defensible combination of:

1. Impact or preserved human priority.
2. Real activity or ongoing work.
3. User-report or operational evidence.
4. Cross-record corroboration within the explicitly selected sources.
5. A sufficiently clear next human decision.

When the user explicitly requests a broader or different population, follow
that request and state how it differs from the default triage.

## Similarity and user evidence

Use more than superficial keyword overlap to relate records. Compare the
problem/symptom, affected component or workflow, timing, expected outcome,
identifiers, and supporting evidence. Keep uncertain overlap visible as
`possible relation`; use `cluster candidate` or `recommended cluster` only
when the rationale supports it.

An explicit shared identifier or source link can establish a **confirmed**
user-report relationship. A consistent match on symptom, context, and timing
may be **probable**. A semantic resemblance without enough support is only
**possible**. Do not convert an inference into a fact.
