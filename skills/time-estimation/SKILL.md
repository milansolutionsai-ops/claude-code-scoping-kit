---
name: time-estimation
description: Use when the user asks "how long will this take", when scoping or quoting any task or project (web/app dev, CRM or no-code builds, automations, integrations), or when about to state ANY duration estimate. Produces dual-track estimates (market billable hours vs your real AI-assisted hours) plus a separate calendar date, and enforces retroactive actuals logging. Pairs with the `scoping` skill, which verifies and packages the work before and after the hours math.
---

# Time Estimation: AI-assisted vs market

**Rule zero: never quote a human-team timeline for AI-executed work. Every estimate is dual-track.**

**Rule one: HOURS and CALENDAR are different questions. Answer both, separately.**
AI compresses hands-on hours a lot. It does NOT compress calendar: scoping, access, client replies, approvals and your own availability don't shrink. Quoting compressed hours as a delivery date is the single biggest source of missed deadlines. Hours drive the value gap. Calendar drives the promise.

This is measured, not a hunch. Across 500k+ GitHub developers, successive AI generations raised commits +30% / +180% / +240% (autocomplete / interactive / autonomous agents) while *releases* rose only **+30%**. Verification, review and human bottlenecks absorb the rest (Demirer, Musolff & Yang, NBER w35275, May 2026). Real builds show the same thing: in one logged MVP, **46% of total project hours landed after "v1 done", spread over 5x the calendar.** Never quote the build number as the done number.

## Settings

Read `config.md` in this folder before estimating. It holds the user's hourly rate, currency, default committed hours per week, and the AI speed-up divisor. If a value is still `SET_ME`, ask once or leave that line out. Never invent a rate.

## Output format (always)

```
TASK: <one line>
Market quote:   <Y> billable hrs (about <Y/25> weeks calendar)  [-> <Y x rate> <currency> only if a rate is set in config.md]
You, hands-on:  <X> real hrs  [range: <X x lo> - <X x hi> per confidence]
You, calendar:  <X / committed hrs per week on THIS project> + external waits -> <date>
Value gap:      <Y/X>x on hours
Confidence:     high / medium / low
Biggest uncertainty: <the one thing that could blow this up>
Bottleneck:     <what actually gates delivery: usually your attention, access, or an external wait>
```

Calendar = hands-on hrs / committed hrs per week on THIS project **+ external waits + queue behind your other live work**. Committed hrs per week: ASK. If the user doesn't say, use the default in `config.md`. Most people run several projects at once, so raw hours never map to "tomorrow". Never state a money figure without a rate from `config.md`. If delivering to a client, quote the calendar line, never the hands-on line.

## Your hands-on hours: how to compute

1. **Decompose the task into concrete build steps.** This is the point of the exercise. List steps explicitly, never a single gut number. Hidden scope surfaces here or not at all.
2. **Sum three buckets separately. They run at different speeds:**
   - **Generated** (code, config, scripts, queries): human-expert effort / the divisor in `config.md` (default 6). The divisor is a placeholder, not a measurement. The log replaces it.
   - **Manual at human speed, NO discount:** clicking through no-code or CRM UIs, interface design, field configuration, anything with no API.
   - **Verification at human speed:** one pass per chunk: preview, test, click the actual flow, client UAT. If you review AI output at gist level, silent failures surface here. This bucket is routinely forgotten and is where the hours actually go.
3. **x 1.5 for omitted scope.** Every estimate omits something (Jorgensen: 30-40% overrun is the human baseline). Applies to your track, not the market quote. Market quotes are already all-in.
4. **Sanity-check against the table.** If more than 2x off, say which bucket drove it.
5. **If 3 or more logged actuals exist for the archetype, the log's median OVERRIDES steps 2-4.** Say so when using it.

## Chunking: the one rule worth keeping from the benchmarks

Agent success decays with task length, and the number that matters is the **80% horizon, not the 50%**. You need work that works, not work that works half the time. METR's measured 80% horizons are **1-3 human-hours even for frontier models**, roughly 5-10x shorter than the headline 50% figures.

**So: chunk any unverified stretch at 2h or less of human-equivalent effort, with a human-verifies checkpoint between chunks.** Checkpoints restore the memory the agent lacks; that is the reason multi-session agentic builds work at all. Longer chunks don't fail loudly. They fail silently and surface in QA or in front of the client.

Do not compute a success probability. The published horizons measure autonomous one-shot runs with an automated verifier; steering an agent interactively every few minutes is a different regime, in an unmeasured direction. The chunk size is the only transferable output.

## Market billable hours (the quote number)

1. **Anchor to the table below.** All-in agency or freelance quotes, already including scoping, revisions, QA, PM, comms, deployment.
2. If you only have raw hands-on effort, inflate: billable = effort x 1.4-1.8 (a dev delivers about 6 productive hours per 8-hour day; planning about 20%; QA 1:3-4 of dev; revisions).
3. Turn hours into money only with the rate in `config.md`.
4. Always a RANGE. Market quotes span 2-4x for the same brief.
5. Don't add an overrun multiplier here; it's already priced in.

## Reference table

Market column = typical all-in quote hours. **AI-assisted column = an unsourced prior from the author's own builds, overwritten by your log the moment 3 actuals exist for a row.** Both columns are hands-on hours, not calendar.

| Archetype | Market (billable hrs, all-in) | AI-assisted hands-on hrs (prior) |
|---|---|---|
| Script / one-off utility / data transform | 4-12 h | 0.25-1 h |
| Web app page or feature (existing repo) | 16-40 h | 1-3 h |
| Landing page (custom) | 20-60 h | 2-6 h |
| Small marketing website | 80-150 h | 8-16 h |
| API integration (webhook, CRM sync) | 16-40 h | 1-4 h |
| Working MVP web app | 300-600+ h | 17-20 h to v1; about 33 h to stable |
| No-code database + automations (3-6 tables, 5-8 automations) | 40-80 h | 2-8 h build + manual UI at human speed |
| CRM funnel / automation setup | 10-40 h | 1-3 h per workflow |
| Debugging in a large codebase you know and the model doesn't | baseline | poor AI fit: 0.8-1.2x human time, no speed-up |

## Confidence -> a spread, not a label

Print the range, every time. A label alone changes nothing the reader does.

Once an archetype has 5 or more logged actuals, **use the empirical median as P50 and the 80th percentile as P80** and drop the multipliers. Until then these multipliers are an *uncalibrated prior*, not a measurement. Say so when you use them:

- **High** (archetype has 3+ logged actuals, all access in hand): x0.7 - x1.5
- **Medium** (matches a table archetype, no actuals): x0.5 - x2.5
- **Low** (novel, unfamiliar system, or blocked on an external party): x0.3 - x4, and name the blocker

## Logging actuals: retroactive, not ritual

A live-clock version of this never gets done. Log at the point work is marked done or handed off, reconstructing times from task-tracker timestamps, git log, or session dates. Don't require the estimating session to also be the finishing session.

Append a row to `calibration-log.md` (same folder):
`| date | archetype | market est | your est hrs | your actual hrs | elapsed calendar days | sessions | what the estimate omitted |`

The omissions column is the valuable one. It is what calibrates the x1.5 and the verification bucket. Backfill past work in one sitting when asked; 5-8 entries makes the override real.

When you estimate before the work starts, add the row to the "awaiting actuals" table in the log, and fill the actual columns when the work closes.

## Provenance

- **METR horizons:** `metr.org/assets/benchmark_results_1_1.yaml`, dataset 2026-05-08. METR's own note: measurements above 16h are unreliable with the current task suite. No model-pinned constant lives in this file; it would go stale in one release cycle.
- **Why no probability arithmetic:** METR fits a **logistic** curve to derive p50/p80. Ord's half-life model (arXiv 2505.05115) is a **different, exponential** model that may not generalise across task suites. Mixing the two, and feeding them "keyboard time" where METR measures full task completion, stacks three mismatches. Deliberately left out.
- **Regime mismatch:** all horizon data is autonomous one-shot runs with an automated verifier. Interactive steering is different. Chunk size is the only output that transfers.
- **Shipped vs written:** Demirer, Musolff & Yang, "Writing Code vs. Shipping Code", NBER w35275 (May 2026).
