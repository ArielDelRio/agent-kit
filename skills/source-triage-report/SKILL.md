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
- Interpret a natural-language request into a query plan: source, requested
  scope, filters, time range, limit, and detail level. Use it to make the
  review reliable, but do not turn it into a report section. Mention a scope
  limit only where it materially changes a finding or confidence.

## Collect and analyze

1. Before a substantive query, make a small read-only reconnaissance query when
   the source is custom, unknown, or its available fields/states are unclear.
   Use it to discover the actual schema, status/state vocabulary, priority
   signals, dates, identifiers, links, and approximate volume. Do not invent
   what “active” or “urgent” means; derive and document the criteria used from
   what the source exposes.
2. Execute the requested query with pagination where available. Honor a user
   limit. For a broad request without a limit, collect up to 500 records per
   source. Record any truncation, inaccessible fields, or failed pages. Put
   those facts in the report only when they materially change a finding or its
   confidence. Use selective fields and compact pages where the source
   supports them; do not load raw payloads or verbose nested material before
   it is needed.
3. Choose the requested detail level, or use **summary** by default:
   - **summary**: inspect compact evidence for the full collected scope, then
     retrieve enough additional context automatically for candidates, clusters,
     and ambiguous cases. Aggregate clearly irrelevant exclusions rather than
     reproducing each record; retain a query, count, and source location that
     make the coverage traceable.
   - **detailed**: do the same, with expanded evidence for every candidate and
     related record, plus richer context for material exclusions.
   - **full**: retrieve and present all relevant accessible record detail for
     the collected scope. It is not a raw source export: paginate in bounded
     batches, select useful fields, and summarize repetitive or oversized
     nested material while retaining its count, representative evidence, and
     source link.
   A detail level changes report depth, not the requested record scope or its
   limit. Do not require a follow-up request to expand records that materially
   affect a conclusion. When the user requests one or more identified tickets,
   lead with their current state, reported problem, impact, evidence, relevant
   context, and next decision—not collection mechanics or generic ranking.
4. Normalize only the information needed for the selected level: stable ID,
   title/summary, URL or source location, state, priority, timestamps,
   owner/work signals, user-report evidence, and relevant context. Preserve
   useful evidence in the report: provide enough excerpt or detail to review
   the conclusion without opening the source where practical; summarize only
   when volume makes that necessary.
5. When the host supports subagents, give one a read-only analysis task over a
   compact normalized index and the expanded evidence for selected records. It
   may classify candidates and propose relations or clusters, but it must not
   make source calls or perform mutations. Do not duplicate raw source output
   into its context. If delegation is unavailable, perform the same analysis
   directly; do not report the implementation detail.
6. Prioritize activity, impact, original human priority, evidence of user
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

Write one Markdown report that presents findings, not a query audit, at the
user-provided location. Otherwise use
`source-triage-report-YYYY-MM-DD.md` in the current directory; if it exists,
add a time or numeric suffix rather than overwriting it.
