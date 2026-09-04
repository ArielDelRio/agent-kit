# Worked example

A complete report, anonymised. Issue keys are `HED-0001` upward so they cannot
be mistaken for real ones, and no person is named except the owner placeholder
`<Owner Name>`.

Read it for **shape and voice**, not content. Note what it does not contain: no
commit count, no colleague's work, and nothing open sitting in a "shipped"
table.

This example is a **modest day** on purpose. One thing merged, several things in
review, one decision owed. Most days look like this, and the report should not
strain to sound busier than the day was.

---

```markdown
# Hedgestone Daily Progress Report

**Date:** March 12, 2026
**Covers:** 11 Mar
**Prepared for:** Hedgestone Business Advisors
**Prepared by:** <Owner Name> - QWave Labs

---

## Executive summary

One fix reached production and three more finished review. The day's work was
mostly closing out the deal-stage refusal cluster: the last two surfaces now
give brokers a reason instead of a bare server error, which was the original
complaint behind two support tickets.

One thing did not go to plan. The seller-side fix passed every check and still
cannot ship, because the data it reads was never backfilled in production. That
is recorded below rather than counted as delivered.

**Housekeeping:** 1 PR merged, 2 PRs opened, 1 PR closed, 1 issue closed,
2 issues filed, 1 defect still live in production.

---

## Shipped to production — 1 issue

| Issue    | PR    | What it fixes                                                                                                                                                                                                                                     |
| -------- | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| HED-0001 | #2101 | Moving a deal to a stage the broker could not write to failed with a bare server error, so nobody knew whether the move had saved. The refusal now names the stage and the missing permission, and the board no longer shows the move as applied. |

---

## Closed without merging — 1

| PR    | Why                                                                             |
| ----- | ------------------------------------------------------------------------------- |
| #2094 | Superseded by #2101, which covers the same refusal path with the shared helper. |

---

## Opened — 2

| Issue    | PR    | What it does                                                                                       |
| -------- | ----- | -------------------------------------------------------------------------------------------------- |
| HED-0002 | #2108 | Seller board hides records whose owner has left, so their book looks empty to whoever inherits it. |
| HED-0003 | #2109 | The export button is offered to brokers who cannot export, and the failure is silent.              |

---

## In review — not shipped

Complete and passing, but **open pull requests, not production changes**.
Nothing here has reached a broker.

| Issue    | PR    | State                              |
| -------- | ----- | ---------------------------------- |
| HED-0002 | #2108 | Open, checks green                 |
| HED-0003 | #2109 | Open, checks green                 |
| HED-0004 | #2088 | Draft — awaiting a business ruling |

---

## Meetings

| Time        | Meeting                 | Outcome                                                                                                        |
| ----------- | ----------------------- | -------------------------------------------------------------------------------------------------------------- |
| 09:30–10:00 | Weekly engineering sync | Agreed the export work waits until the refusal cluster is fully merged, to avoid two people in the same files. |
| 14:00–14:20 | Support handover        | Two tickets traced to HED-0001; both can be closed once it deploys.                                            |

---

## Issues filed — 2

| Issue    | Why it exists                                                                                                   |
| -------- | --------------------------------------------------------------------------------------------------------------- |
| HED-0005 | The buyer side has the identical refusal defect; deliberately out of scope for HED-0001, which was seller-only. |
| HED-0006 | The backfill that HED-0002 depends on has never run in production. **Blocked on a data run.**                   |

---

## Awaiting a decision

1. **HED-0004** — whether departed owners keep read access to their old book.
   The issue asks to remove it; the implementation keeps it and marks it. A
   finished pull request waits on this, and support evidence points both ways.

---

## Honest accounting

- **HED-0002 will change nothing on merge.** The table it reads is empty in
  production because the backfill was never run. Filed as HED-0006.
- **I reported HED-0003 as ready yesterday; it was not.** The check I trusted
  had run against an older commit. Re-checked and corrected today.
- **A near-miss worth recording.** A test that looked like it covered the export
  permission could not fail, because the collection it asserted over is empty in
  that scenario. Repaired, and two similar tests found in the same file.

---

## Next

Merge order matters in one place: **#2108 before #2109**, since the second
builds on the first.

The decision on HED-0004 is the critical path; nothing else is blocked.

---

_Prepared by <Owner Name> for QWave Labs, 11 March 2026._
```

---

## What to copy from it

- **The header and footer, verbatim** in shape. Footer date is the covered day,
  not the writing day.
- **Shipped means merged.** One row here, because one thing merged.
- **Every table leads with the issue**, and the description is the business
  problem, not the diff.
- **Honest accounting names the owner's own errors**, in the first person, not
  in the passive voice.
- **Meetings carry times** because the time is the fact. Every other table has
  no time column.
- **A short report for a modest day.** No padding, and no colleague's work
  borrowed to fill it out.
