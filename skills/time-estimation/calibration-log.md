# Calibration log: estimate vs actual

One row per completed job. Fill it retroactively, when work is marked done or handed off.
Reconstruct times from git log, task-tracker timestamps, or session dates. Don't wait for a
live clock; that version of this ritual never gets done.

The **omitted** column is the valuable one: it calibrates the x1.5 scope multiplier and the
verification bucket. Once an archetype has 3 entries its median overrides the reference table
in `SKILL.md`. At 5, use the empirical median as P50 and the 80th percentile as P80, and drop
the confidence multipliers.

If you backfill from commit history, a simple method: group commits into sessions with a gap
of more than 3h as the split, and sum each session's first-to-last commit span. It misses
thinking time that produced no commit and overstates sessions that idled, so treat those rows
as plus or minus 30%.

**The two rows below are made-up examples. Delete them once you add your first real row.**

| Date | Archetype | Market est | Your est (hrs) | Your actual (hrs) | Elapsed days | Sessions | What the estimate omitted |
|---|---|---|---|---|---|---|---|
| 2026-01-15 | EXAMPLE: Landing page (custom) for a fictional bakery | 20-60h | 4 | 6.5 | 9 | 3 | Copy arrived late and needed two rounds; mobile QA on real phones took longer than the build. |
| 2026-02-03 | EXAMPLE: API integration (form -> CRM sync) for a fictional gym | 16-40h | 2 | 3.5 | 5 | 2 | CRM field names differed from the spec; one retry path for failed webhooks was not in scope but needed. |

## Estimates on record, awaiting actuals

Estimates stated **before** the work. Fill the actual and omissions columns as each one closes.
This is the only way to learn whether your estimates are biased, not just noisy.

| Date est. | Archetype | Market est | Your est (hrs) | Your actual | Elapsed days | Sessions | What the estimate omitted |
|---|---|---|---|---|---|---|---|
