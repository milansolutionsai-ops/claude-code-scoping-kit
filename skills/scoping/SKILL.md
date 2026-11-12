---
name: scoping
description: Use when scoping work for a client or project: a feature-request list arrives, "what would it take to...", "how much work is this", "put together a plan/proposal/SOW", "scope this out", batching work into monthly blocks, or renegotiating what's in and out. Covers custom code and CRM/automation work primarily; ads/branding/marketing secondarily. Forces verification against the live system BEFORE any estimate, and produces two artifacts (internal with hours, client-facing without). Pairs with the `time-estimation` skill, which owns the hours math.
---

# Scoping: verify first, estimate second

## The core bug this skill fixes

The default failure is **scoping from the brief instead of from the system**. A client sends a list, the list sounds reasonable, and estimates get written against the list. Every item is then priced as if it were net-new work, half the "bugs" are misunderstandings, some items are already built, and one or two are impossible as described.

Industry guides say "do technical discovery" without saying what that means. This skill says exactly what it means: **open the thing and look.**

**Rule zero: never estimate an item whose current state you have not verified against the live system.** Not the brief, not the client's summary, not your own memory of the codebase, not a doc a teammate wrote. The system.

This is not optional and not a time-saver to skip under pressure. Verification is what makes the estimate worth anything.

---

## The five steps

1. **Verify** every claim against the live system (the gate)
2. **Classify** each item by cost profile
3. **Find the real cost driver** (mechanism vs configuration)
4. **Package** to capacity
5. **Ship two artifacts**: internal and client-facing

---

## Step 1: Verify (the gate)

Four kinds of claim show up in a brief. Each is verified differently.

| Claim type | Sounds like | How to verify |
|---|---|---|
| **Delivery** | "this was never completed" | git log / PR history / deploy history |
| **Behaviour** | "X is broken", "it shows the wrong thing" | read the actual handler or component; trace the data path |
| **Absence** | "we need you to add X" | grep for X first; it often exists under another name |
| **Data** | "the records are wrong / we lost files" | query or list the real store; count things |

### Verification by domain

**Custom code (primary).**
- Delivery claims: `git log --all --date=short` across the relevant repos. Dates settle arguments.
- Behaviour claims: read the function that actually runs. Not the component name, the handler.
- Absence claims: grep the type definitions and the schema before agreeing to build a field.
- Data claims: list or query the real store (bucket, DB, collection). Counts and sizes, not impressions.
- If the repo has a symbol index or code-navigation tool, query symbols rather than re-reading trees.

**CRM and automation platforms (primary, equally).**
- Open the actual account: pipelines, stage names, workflow names, custom fields, tags.
- Verify the **names** the code depends on. App-to-CRM integrations often bind to exact pipeline and workflow names; a rename breaks them silently. List them explicitly before scoping any CRM reorganisation.
- Check whether an automation exists and is **published**, not just drafted.
- Establish sync direction. One-way app-to-CRM means any control enforced in the app can be bypassed by editing the CRM directly. Say so out loud; clients usually assume two-way.
- Custom-field mapping files in the codebase are the source of truth for what actually syncs.

**Ads / branding / marketing (secondary).**
- Pull the real account: campaign structure, spend, conversions, naming conventions in use.
- Check what's actually live vs paused vs drafted before scoping "optimisation".
- For branding, find the current assets and guidelines before scoping a refresh.

### The verification writeup

For each item, record one line of evidence with a pointer: `file:line`, a commit date, a stage name, a row count. If you cannot produce that pointer, the item is **unverified** and must be labelled as such in the internal doc.

**Expect to be wrong, including about your own prior claims.** Verification that only ever confirms you was not verification. When the code overturns a confident assertion you already put in a client document, that is the process working, not failing.

---

## Step 2: Classify by cost profile

Never present a flat list. Four buckets, four different cost treatments:

- **Already built**: verified working. Cost: zero, plus the conversation explaining it. Frequently the highest-value finding in the whole exercise.
- **Partly built**: the data, endpoint, or value already exists; only the surfacing is missing. Cost: a fraction of net-new. Say which fraction and why.
- **Net new**: genuinely absent. Full estimate.
- **Blocked**: cannot be estimated or built until someone decides or supplies something. Name the blocker AND its owner. Never let a blocked item sit in a list looking like work.

A brief that arrives as "11 features" routinely resolves to a handful of net-new items, several surfacing jobs, one or two already done, and a couple of decisions disguised as features.

---

## Step 3: Find the real cost driver

Before estimating, ask: **what actually drives the hours here?**

The most common trap is **mechanism vs configuration**. Building an approval workflow for 8 fields costs almost exactly what it costs for 25 fields. The mechanism is the work; the field list is config. Same shape: number of users, number of orgs, number of campaigns. Usually config.

Two payoffs:
1. You stop over-estimating things that look big because a list is long.
2. **You can scope confidently while a key input is still missing.** If the field list is late, say so and proceed; the estimate does not depend on it. This unblocks the whole engagement instead of stalling it.

The inverse also matters: find the thing that looks small but drives real cost. A data-model change to a field used across a 3,000-line form is not small.

---

## Step 4: Package to capacity

If there is a monthly capacity (hours, budget, or attention), package into **monthly batches sized to that capacity** rather than one undifferentiated list.

- **Sequence pain-first.** Anything currently broken goes in batch one. Cheap goodwill, and it stops the reactive-request cycle that usually triggered the scoping exercise.
- **Keep all-or-nothing work in one batch.** If a system has no visible value until every part works, splitting it across months produces two months with nothing to show.
- **Estimate at the upper end** when committing to a batch. Under-filling a month is recoverable; overrunning is not.
- **Leave the last batch light.** It absorbs overruns from earlier ones.
- **Note cross-batch dependencies and external blockers** against the batch they threaten.

### Track hours internally, commit deliverables externally

Hours-based blocks have a known failure mode: clients who underuse the block invent work to fill it, clients who overrun feel constantly billed extra, and the monthly hours-used-vs-available negotiation becomes a tax on the relationship. Deliverable-framed months avoid it: the conversation becomes "did we deliver what we said" instead of "how many hours did we burn".

So: **hours live in the internal doc; the client-facing doc commits to named deliverables per month.** If the commercial deal is denominated in hours, that belongs in the contract, not in the scope document.

---

## Step 5: Ship two artifacts

**Never one document for two audiences.** That is the sales/delivery mismatch in physical form.

**Internal doc**: hours (both tracks), cost profiles, evidence pointers, blast radius, technical risks, dependency warnings, decisions still open, and anything commercially sensitive.

**Client-facing doc**: no hours. Plain language, no internal jargon, organised by month, each item stating what changes for them. Complexity labels only if they help the reader decide.

Keep both in sync. If the internal doc changes materially, the client-facing one is stale.

Skeletons for both are in `templates.md` in this folder.

---

## Required sections (the ones everyone skips)

These separate a scope from a wish list. Each maps to a known failure mode.

- **Exclusions, with reasoning.** What you are explicitly NOT building, and why. Naming a rejected approach and its reason prevents it being re-litigated every month. If you rejected the obvious solution, explain it in the client doc; they will otherwise assume you missed it.
- **Acceptance criteria.** What "done" looks like per item. Skipping this is what produces "we thought that was complete" arguments later. Especially critical where a prior delivery was already disputed.
- **Change control.** What happens when something new is requested mid-engagement. Unbilled out-of-scope work is a structural margin leak that one paragraph prevents.
- **Decisions, each with a recommendation.** Never ship a bare open question. A recommendation converts an unchallenged assumption (the top scope-creep cause) into something someone must actively reject. State the recommendation and the reasoning.
- **Blockers, each with an owner and an impact.** Which item stalls, and who unblocks it.
- **Verified-state notes.** Anything you checked that contradicts the brief, or that reassures. "We audited X and nothing was lost" is often the single most valuable line in the document.

---

## Prioritisation

Keep it light. Two tools, different jobs:

- **MoSCoW** (Must / Should / Could / Won't): for deciding what's in a release or a month. Fast, categorical, needs no data. This is the default.
- **RICE**: only when ranking a large backlog and stakeholders need a numerical justification. Overkill for a client feature list.

Combine only if the list is big: MoSCoW to set the boundary, RICE to order inside it.

The **Won't** column earns its keep. It is where exclusions come from.

---

## Hours

**This skill does not compute hours.** Invoke the **`time-estimation`** skill for that. It owns the dual-track math (AI-assisted vs market billable), the decomposition step, the calendar date, and the calibration log.

Sequence: verify and classify here, estimate there, package and write up here.

---

## Red flags: these thoughts mean stop

| Thought | Reality |
|---|---|
| "The client described it clearly, I can estimate it" | The description is a symptom report, not a spec. Verify. |
| "I remember how that part of the codebase works" | Memory is stale and confidently wrong. Open it. |
| "Our own doc says we shipped that" | Docs drift from reality. Check git. |
| "It's obviously not built yet" | Grep first. Absence claims are wrong surprisingly often. |
| "The list has 20 fields so it's 20 units of work" | Find the cost driver. It's usually the mechanism. |
| "I'll note the open question and move on" | Bare questions stall for weeks. Attach a recommendation. |
| "I can't scope this until they send the missing input" | Check whether the estimate actually depends on it. Usually not. |
| "One doc is enough, I'll just leave the hours in" | That's how internal numbers reach the client. Two artifacts. |
| "Exclusions feel negative, I'll leave them out" | Unstated exclusions get re-requested monthly forever. |
| "We'll sort out 'done' when we get there" | That is the argument you are being asked to scope *because of*. |

---

## Field-tested evidence

The rules above came out of a real client engagement (details anonymised). Every rule earned its place:

- Four contract deliverables were marked "not completed" by the client. **git history showed all four shipped**, late but delivered. The entire framing of the engagement changed.
- A "payments tagged with the wrong type" bug traced to a catch-all handler, with the relevant fix dated **one day after** the affected record. A historical data cluster, not a live bug.
- "Improve the search" turned out to be a screen that already had a searchable dropdown. The real ask was a different screen.
- "Add a document field" already existed under another name. One question removed from the client doc.
- "Documents are at risk of being lost": a storage audit found **over a thousand files, nothing ever overwritten, one field 24 versions deep**. The requested feature was already half-true. Became the strongest trust item in the deliverable.
- A confident claim about a payment-provider limitation, already written into a client document, was **wrong**, and the code proved it. Corrected before it shipped.
- A 22-field list arrived late. The estimate didn't depend on it: the approval mechanism was the cost, not the fields. Scoping proceeded on schedule.

---

*Companion: `templates.md` in this folder has skeletons for both artifacts. Process outranks template; if the two conflict, follow the process.*
