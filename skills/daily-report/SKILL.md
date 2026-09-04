---
name: daily-report
description: "Write the HedgeStone daily engineering report on explicit request. The report is written the morning AFTER the day it covers, so today's file covers yesterday; on a Monday it covers Friday through Sunday as one range. Gathers the real record from git, GitHub PRs and Linear rather than from memory, then writes to .serena/docs. Use when the user asks for the daily report, yesterday's report, the standup writeup, or a catch-up covering the weekend."
disable-model-invocation: true
---

<!--
Maintainer note. This skill is deliberately not portable: it names a specific
client (HedgeStone Business Advisors), a specific vendor (QWave Labs), a
specific Linear project and issue prefix, and a local output path under
`.serena/docs`. Publishing that in a public repository conflicts with the
public-repository boundary in AGENTS.md.

The recommendation is to strip the company names and every organization-specific
reference, or to parameterize them. It is kept as-is for now because the
intention is for this skill and the project it serves to move to QWave Labs'
private, secured repository, where client-specific detail belongs.

Until that move happens, do not add anything further here that is not already
public: no credentials, no customer data, no live service configuration.
-->

# Daily report

Run this skill only when the user explicitly asks for the daily report. Do not
invoke it automatically from surrounding discussion, from a day's work being
finished, or from an available git, GitHub, or Linear connection.

**Read [`references/example-report.md`](references/example-report.md) before
writing your first one.** It is a complete anonymised report showing the exact
header, every section, the tables, and the footer. Copy its shape; it is faster
and more reliable than assembling the format from the rules below.


## THE REPORT COVERS ONLY THE OWNER'S OWN WORK

This is the rule that gets broken most often, and breaking it makes the report
useless. **Every merge, commit, issue and decision in the report must be the
report owner's own.** Other people's work does not appear — not as context, not
as a "filed in parallel" aside, not in a shipped table, not anywhere.

If a teammate merged eleven PRs the same day, none of them belong in this
report. If a teammate filed fifteen issues, none of them belong either. The
reader wants to know what the owner did.

**So every gathering command must filter by author or assignee.** Never run an
unfiltered query and then eyeball the result — you will pull in a colleague's
work and it will read as if the owner did it.

The only exception is a dependency the owner is blocked on, and it is named as
someone else's: "waiting on a ruling from X", never listed as work done.


One report per working day, written the morning after. It is a record of what
actually happened, not a summary of intentions.

## Step 0: work out which day you are covering

**The filename carries the day the report is WRITTEN. The body covers the day
BEFORE.** Get this wrong and the report silently describes the wrong day.

```bash
date +%Y-%m-%d          # today — this is the filename date
date -v-1d +%Y-%m-%d    # yesterday — this is what you cover (macOS)
date +%A                 # today's weekday, which decides the coverage window
```

| Today is        | File                                   | Covers                                        |
| --------------- | -------------------------------------- | --------------------------------------------- |
| Tue–Fri         | `daily-report-<mon>-<dd>.md` for today | yesterday, one day                            |
| **Monday**      | `daily-report-<mon>-<dd>-to-<dd>.md`   | **Friday, Saturday and Sunday**               |
| After a holiday | range file                             | every non-reporting day since the last report |

The Monday case is not optional. Friday's work gets no separate report of its
own. Monday's report carries Friday, plus a separate one for the weekend if that
applies. Check `.serena/docs/` for the last report written and cover **every day
since**, so nothing falls in a gap.
`HedgeStone daily-report-aug-28-to-31.md` is the worked example.

**Use local dates, not UTC.** Linear and GitHub timestamps are UTC; the working
day is local. A commit at 20:02 local is 00:02 UTC the next day, and filing it
under the wrong day is the most common error in this report.

## Step 1: gather the record — never write from memory

Run all of these for the coverage window. Redirect long output to a scratch
file and read it back; Bash output here is compressed and drops content.

**Every command below is filtered to the owner.** Confirm the owner's git
author and GitHub login first, and do not drop the filters.

```bash
# Commits — ONLY the owner's. Use these to learn WHAT was worked on and
# which issues moved. Never report a commit count; see the rule below.
git fetch --all --quiet
git log --all --no-merges --author="<owner git author or email>" \
  --since="<start>T00:00:00" --until="<end+1>T00:00:00" \
  --pretty=format:"%h|%ad|%s" --date=format:'%H:%M'

# PRs the owner MERGED in the window
gh pr list --state merged --limit 60 --author <login> \
  --json number,title,mergedAt \
  --jq '.[] | select(.mergedAt >= "<start>T00:00:00Z" and .mergedAt < "<end+1>T00:00:00Z")'

# PRs the owner OPENED in the window
gh pr list --state all --limit 80 --author <login> --json number,title,createdAt

# The owner's open PRs and their real state
gh pr list --state open --limit 40 --author <login> --json number,isDraft,title
```

Linear, via MCP: `list_issues` with `assignee` set to the owner, `updatedAt` at
the window start, `project: "CRM v1.0"`. Separate **completed in the window**
from **created in the window** — they tell different stories. Issues assigned to
anyone else are out of scope even if they are related.

Read the previous report before writing. It sets the running narrative, and a
claim it made may have since been disproved.

## Step 2: verify before you assert

- **Never restate a PR's status from a prior report.** Run `gh pr view <n>`.
- A Linear issue marked Done is not proof the defect is fixed. Several here were
  closed on merge evidence for migrations never executed in production.
- Distinguish **merged** from **resolved**, and say which you mean.
- If a number is inherited rather than re-pulled, mark it or leave it out.

## Step 3: write it

Target 150–230 lines. Sections, in order:

1. **Header** — reproduce this block exactly, it is the house format:

   ```
   # Hedgestone Daily Progress Report

   **Date:** <Month D, YYYY>            <- the day you are writing
   **Covers:** <D Mon>                  <- the day(s) covered
   **Prepared for:** Hedgestone Business Advisors
   **Prepared by:** <Owner Name> - QWave Labs
   ```

   `Prepared for` is always Hedgestone Business Advisors. `Prepared by` is the
   owner's name, then a hyphen, then the company: `Arlan Galvez - QWave Labs`.
   Never omit either line, and do not write the company alone.

   **The report also ends with a footer**, as the last line of the file:

   ```
   ---

   _Prepared by <Owner Name> for QWave Labs, <D Month YYYY>._
   ```

   Note the footer date is the **day the report covers**, not the day it is
   written. A report written on 4 September covering 3 September ends
   `_Prepared by Arlan Galvez for QWave Labs, 3 September 2026._`
   On a Monday range report, use the last day covered.

2. **Executive summary** — one or two paragraphs, headed exactly
   `## Executive summary`. What the day amounted to, not a count. A day of 100
   work that mostly proved existing work wrong should say so. Close it with a
   one-line `**Housekeeping:**` tally: PRs merged, PRs opened, PRs closed,
   issues closed, issues filed, defects still live. **No commit count.**
3. **Shipped to production** — see the table rule below.
4. **The body of the day** — whatever the work actually was. Tables for
   anything with more than three rows.
4b. **Meetings** — see the rule below.
5. **Issues filed** — with why, and which are blocked.
6. **Awaiting a decision** — numbered, each with the exact question owed and who
   owes it. This is the section people act on.
7. **Honest accounting** — see below.
8. **Next** — merge order, sequencing constraints, the critical path and the things you plan to do on the current day if applies.

## Never report commit counts

Commit counts are not an achievement and they do not belong in this report. Do
not put a number of commits in the summary, in the housekeeping line, in a
table, or anywhere else. Do not add a commits-per-issue breakdown.

A commit count measures how work was chopped up, not what was delivered. Ten
commits fixing one defect and one commit fixing ten read identically as a
number, and the number invites the reader to treat activity as progress.

Use `git log` to learn **which issues moved and what changed**, then report the
outcome: what shipped, what is in review, what was found. If a day produced a
lot of work with nothing merged, say that in words and explain why.

## Meetings — always ask, never invent

Meetings do not appear in git or Linear, so the record cannot find them. **Ask
the owner** before finishing the report:

> Any meetings on <the covered day> to include? If so, send me the time, who was
> in it, and a line on what came out of it, and I will add them.

If the owner supplies them, add a `## Meetings` section placed after the body
and before the issues-filed section:

```
| Time | Meeting | Outcome |
| --- | --- | --- |
| 10:00–10:30 | Weekly sync with <who> | <what was decided or agreed> |
```

Meetings are the one place a **time column is correct**, because the time is the
fact being recorded. Note the duration, not just the start, since the report is
partly a record of where the day went.

If the owner has none, or does not answer, **omit the section entirely**. Never
invent a meeting, never guess at attendees, and never infer one from a calendar
you cannot read. An absent section is accurate; a fabricated one is not.

## Every table leads with the issue, never the time

The reader cares which business problem was solved, not what o'clock a branch
was cut. Timestamps belong in the gathering step, not the report.

```
| Issue | PR | What it fixes |
| --- | --- | --- |
| HED-1444 | #1332 | Plain-English statement of the defect and what now happens. |
```

- **Issue key first**, then the PR number, then the effect. If one PR closes
  several issues, list them all in the first cell.
- **Describe the defect in business terms**, not the diff. "Readers were offered
  a composer they had no authority to use, and the refusal discarded their typed
  note" beats "gated composer on projection".
- **No time column.** Not merge time, not creation time.
- A PR with no issue still gets a row; put the reason in the issue cell.

## Merged means merged on the covered day

A PR belongs in **Shipped** on the day it actually merged, and nowhere else.

This is the most common error in this report, because a PR opened one day and
merged the next tempts you to record it in the earlier report. Do not. The
earlier report may list it as sent for review; the merge is reported on the day
it happened, under **Shipped to production**.

Check merge dates rather than trusting the previous report:

```bash
gh pr view <n> --json number,title,mergedAt
```

If yesterday's report claimed something shipped that had not merged yet, say so
in today's honest accounting and record it correctly here.

## The honest-accounting section is mandatory

Every report carries it, and it is the reason anyone trusts the rest. Include:

- Work that **merged but did not resolve** its issue, and why.
- Work that is **inert in production** — code that lands but changes nothing for
  a user until a migration, flag or data run happens.
- **Red checks that are not real failures**, and green ones that prove less than
  they look like.
- **Your own errors**, named plainly. Instructions that were wrong, claims
  repeated without checking, things that had to be corrected. A report that only
  contains other people's mistakes is not being honest, it is being defensive.
- **Near-misses** worth knowing about even though nothing broke.

Write "I was wrong about X" rather than "X was clarified". The passive voice is
how a report stops being useful.

## Voice

- Lead with the outcome. State facts and let them carry the weight.
- One idea per sentence. No em-dashes, no parentheticals.
- Numbers go in tables, not in prose.
- Bold the first few words of a bullet, never a whole sentence.
- Do not invent a metric. If a figure was not measured in the window, say where
  it came from and when.
- Name a file, function or issue only where the reader has to go there.

## Output

Write to `.serena/docs/HedgeStone daily-report-<mon>-<dd>.md`, matching the
existing filenames exactly (lowercase three-letter month, zero-padded day).
`.serena/` is gitignored and must stay that way — **never** write these into
`docs/`, never `git add` them, and never let one reach a PR.

## Do not

- Do not run `pnpm typecheck` or `pnpm lint` to produce this report; it is a
  record, not a gate.
- Do not call `mcp__convex-prod-ro__*` — every call hangs. Route production
  questions to a human at the Convex dashboard.
- Do not write to PostHog or Linear. Reading is fine; this report proposes, a
  human acts.
- Do not pad. A quiet day gets a short report, and saying a day was quiet is a
  finding.
