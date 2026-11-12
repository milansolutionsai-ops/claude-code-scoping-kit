# Scoping: artifact skeletons

Reference only. **The process in `SKILL.md` outranks these.** Drop sections that don't apply; never keep a section as an empty ritual. Adapt tone and structure to the client. A standardised-looking document reads as a standardised-looking effort.

---

## A. Internal scope doc (hours, risks, evidence)

Audience: you, your team's leadership, whoever negotiates commercials. Never sent to the client.

```markdown
# <Client>: Development Scope (INTERNAL)

**INTERNAL: hours and commercial notes. Not to be sent to the client in this form.**
Prepared <date> by <author>

## 1. How to read this
What's being scoped, and the two estimate tracks (from the time-estimation skill):
- AI-assisted: how we actually build it, incl. review and testing
- Market billable: what a contract dev or agency would quote all-in (comparison only)
Ranges, not points. State whether batches use the upper or mid estimate.

## 2. Totals
| BATCH / PILLAR | AI-ASSISTED | MARKET BILLABLE |
Plus: total, and what the billable figure equates to in contract-dev months.

## 3. Batches
One table per batch: REF | ITEM | AI-ASSISTED | MARKET BILLABLE, with a subtotal row.
Note the sequencing logic and anything all-or-nothing.

## 4. Verified state
What was checked, and what contradicted the brief. Evidence pointers
(file:line, commit dates, counts). Flag anything still UNVERIFIED.

## 5. Cost profiles
Already built / partly built / net new / blocked: which items fall where.

## 6. Design decisions taken
Architecture calls already made, so they aren't re-opened next session.

## 7. Decisions needed
D1..Dn, each with a RECOMMENDATION and its reasoning.

## 8. Open questions for the client
Numbered, each noting which item it blocks.

## 9. Notes and risks
Blast radius, integration name dependencies, external blockers,
what's assumed, what breaks if an assumption is wrong.
```

Table headers: **plain CAPS, no bold markers**. Some markdown importers (Google Docs, for one) escape asterisks inside table cells into literal `\*\*text\*\*`.

---

## B. Client-facing doc (no hours)

Audience: the client. Plain language. Deliverable-framed.

```markdown
# <Client>: Development Plan: <scope in their words>

Opening: thank them, confirm we reviewed against the actual platform,
and lead with the answer: what's simple, what's already partly built,
what we already diagnosed.

## How we will deliver this
Monthly deliveries. Why fixes are front-loaded. What each month ends with.

## Month 1: <theme>
**<Item name>.** What it is, in their language, and what changes for them.
(No hours. No internal jargon. No ticket refs.)

## Month 2..N: <theme>
Same shape.

## On <the contentious item>
Where the obvious approach is wrong, explain it: what already exists,
what's actually missing, what we'll build, and why not the obvious thing.
Prevents re-litigation.

## New feature: <the one needing a decision>
The problem. Our proposal. What we need from you.

## How <the new system> will work
Plain-language walkthrough. What they see, what their team sees.

## What we verified
Reassurance items. "We audited X, nothing was lost" is often the
highest-value line in the document.

## What's not included
Explicit exclusions with reasoning.

## What "done" looks like
Acceptance criteria per month or per item.

## If something new comes up
Change control: how mid-engagement requests are handled.

## What we need from you
Questions (numbered) and confirmations (numbered). Close with what
unblocks the start.
```

---

## C. Decisions register (optional)

For engagements with many open calls, or where decisions keep getting re-opened.

| REF | DECISION | RECOMMENDATION | OWNER | STATUS | DATE SETTLED |

Move settled rows into "Design decisions taken" in the internal doc so they stop being questions.

---

## Branded PDF (client deliverable)

When the client-facing doc ships as a PDF: any PDF library you like (e.g. `reportlab` in Python), the client's real logo asset, brand colour in headings and rules, a footer with document name and page number. If the client works in two languages, build both versions at once; one is always asked for later.

**Always render a page to an image and look at it before delivering.** Text extraction will not reveal a broken logo, a mangled accent, or a header colliding with body text.
