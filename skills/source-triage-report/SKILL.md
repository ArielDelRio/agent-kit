---
name: source-triage-report
description: Analyze explicitly selected read-only work and telemetry sources into an evidence-backed Markdown triage report without changing the sources.
disable-model-invocation: true
---

# Source triage report

Create a local Markdown report that helps a person review the active, important,
and related work found in explicitly chosen sources. This skill is analysis only:
never create, edit, merge, assign, move, comment on, or otherwise mutate any
source record.

Run this skill only when the user explicitly requests a source-triage report.
Do not invoke it automatically from surrounding discussion, an available source
connection, or a perceived need to clean a backlog.

Use it for connected work-tracking, issue, feedback, or telemetry sources such
as Linear, PostHog, Sentry, and GitHub, or for a user-provided source the agent
can already read. Do not use it to configure a source, obtain credentials, or
act on the report's recommendations.

## Scope and access

- Require the user to name at least one source. Query only the named sources;
  do not infer or search additional sources for corroboration.
- A custom source is acceptable only when it is readable with tools already
  available to the agent. Ask for its relevant fields or schema when needed;
  never request, store, or add credentials or live configuration.
- Treat all source operations as read-only. If a named source is unavailable,
  say so. When other named sources are usable, continue and record the gap and
  its effect on confidence.
- Interpret a natural-language request into an auditable query plan: source,
  requested scope, filters, time range, and limit. Run an explicit plan without
  a separate confirmation.

## Collect and analyze

1. Before a substantive query, make a small read-only reconnaissance query when
   the source is custom, unknown, or its available fields/states are unclear.
   Use it to discover the actual schema, status/state vocabulary, priority
   signals, dates, identifiers, links, and approximate volume. Do not invent
   what “active” or “urgent” means; derive and document the criteria used from
   what the source exposes.
2. Execute the requested query with pagination where available. Honor a user
   limit. For a broad request without a limit, collect up to 500 records per
   source and state prominently that more records may exist. Record any
   truncation, inaccessible fields, or failed pages.
3. Normalize only the information needed for review: stable ID, title/summary,
   URL or source location, state, priority, timestamps, owner/work signals,
   user-report evidence, and relevant context. Preserve useful evidence in the
   report: provide enough excerpt or detail to review the conclusion without
   opening the source where practical; summarize only when volume makes that
   necessary.
4. When the host supports subagents, give one a read-only analysis task over
   the normalized evidence. It may classify candidates and propose relations or
   clusters, but it must not make source calls or perform mutations. If
   delegation is unavailable, perform the same analysis directly and note that
   fallback in the report.
5. Prioritize activity, impact, original human priority, evidence of user
   impact, corroboration, and actionability unless the user explicitly asks for
   a different universe. Never silently downgrade a human priority.

Read [source guidance](references/source-guidance.md) before querying a source
whose structure needs interpretation. Read [the report format](references/report-format.md)
before writing the output.

## Evidence and recommendations

- Distinguish observed facts from inferences. Give every candidate and cluster
  a confidence of **high**, **medium**, or **low**, with a concrete reason.
  Priority or a bulk-updated timestamp alone is not corroboration.
- For user feedback or reports, retain source, stable ID or link, date, a
  context-sufficient excerpt or summary, and a relationship classification:
  **confirmed**, **probable**, or **possible**.
- Keep two axes separate: a decision (**retain**, **review**, or **discard**)
  and a relationship (**standalone**, **possible relation**, **cluster
  candidate**, or **recommended cluster**). A cluster is a
  reviewable grouping, never an instruction or permission to merge records.
- Select a cluster representative using evidence quality and actionability;
  break ties with impact or original priority. Include every related record and
  explain why it belongs.
- Surface conflicting source evidence rather than resolving it. Recommend
  **review** when the conflict materially affects the conclusion.

Write one Markdown report at the user-provided location. Otherwise use
`source-triage-report-YYYY-MM-DD.md` in the current directory; if it exists,
add a time or numeric suffix rather than overwriting it.
