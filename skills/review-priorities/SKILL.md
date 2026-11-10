---
name: review-priorities
description: Show what to work on first and why. Use when the user asks which issues matter most, what to look at today, what is most urgent, or why one issue ranks above another, and when they disagree with that order. Covers reading the priority ranking, explaining it, and reporting a better order back to Antimetal.
---

# Review priorities

Antimetal ranks active Issues so the most urgent come first. This skill lists that
ranking, explains it in plain terms, and sends a correction back when the user or the
evidence says the order is wrong. Corrections are how the ranking gets better, so
report them; don't just reorder the list in your answer.

## List the ranking (`search_issues`)

Call `search_issues` with `sort: "priority"`. Filter for open work by default
(`investigating`, `ready_to_fix`), and add `severity`, `environment`, `activeFrom` or
other filters when the user names a scope. `limit` sets how many come back (up to 500
with priority sorting, no cursors). Load more by raising `limit`, not by paging.

The ranking applies, in order:

1. **Severity**: critical and high first, then medium, then low, then unrated.
2. **Assignment**: assigned Issues before unassigned ones of the same severity.
3. **Pace**: accelerating Issues (`accelerating: true`: at least 3 alerts in the last
   6 hours, and more than 1.6 times as many as the 6 hours before) ahead of steady ones.
4. **Recency**: the newest first alert ahead of older ones.

When the user wants a different order for this list, pass `order` with key and
direction pairs, such as `severity:desc,age:asc`. Those keys rank ahead of the
default order. Use `order` for what the user asked to see. Don't use it to replace the
ranking without saying so.

## Explain it

Lead with the top few Issues. For each one, say why it is where it is in the ranking's
own terms: its severity, whether someone owns it, whether alerts are accelerating
(`recentSeedCount` against `priorSeedCount`), and how recent it is. Give the issue
URL. Then pull `get_issue_report` for any Issue the user wants to understand.

Say when the ranking is thin. An unrated Issue sorts last because nothing has assessed
it yet, not because it is minor.

## Report a better order (`submit_ranking_feedback`)

A priority result carries `rankSnapshotId`, the saved copy of the list as you received
it. Call `submit_ranking_feedback` when:

- the user says the order is wrong ("#812 should be above #790, it's a customer outage"),
- you had to reorder the list to answer well, for example because a report shows a
  lower Issue is the real cause of a higher one.

Pass `rankSnapshotId`, the Issue `uuid`s in the right order with the most important
first (name only the Issues that are out of place), and `feedback` that says why. The
why is what makes the correction useful. "Customers cannot check out, the one above it
is a staging warning" is a good why. "Reordered" is not. Send one correction for each
disagreement, and confirm the order with the user first when the order is your
inference and not their words.

## Ranking corrections are not severity rules

A correction says that this list was in the wrong order. It does not change any
Issue's severity or the ranking itself. When the user states a lasting rule, such as
"checkout failures are always critical", use **configure-severity-rules** for that
too. When they want one Issue handled now, use **investigate** or **fix**.

## Report tool problems

If a tool result surprised you, a field you needed was missing, or the task needed a
workaround, call `submit_tool_feedback` once for that problem. Name the tools, say
what you expected and what you got, and what would have helped. Leave out secrets.
