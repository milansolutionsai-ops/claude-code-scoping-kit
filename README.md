# scoping-kit: Claude Code skills for quoting AI-assisted work

AI makes the build fast. Quotes are still priced on the old hours.

So when a client asks "how much for this?", you are stuck between two bad answers. Quote
your real hours and you undercharge for work the market prices at five or ten times that.
Quote the market number and you can't explain where it came from. And either way, the date
you promise is wrong, because AI shrinks the hands-on hours but not the waiting: client
replies, access, approvals, review, and the bugs that only show up after "v1 done".

This kit is three Claude Code skills that make Claude scope and estimate the way AI-assisted
work actually behaves.

## What it does

1. **Two numbers per estimate, plus a date.** Every estimate shows the market quote (what
   an agency or freelancer would bill, all-in) next to your real hands-on hours with AI,
   and a separate calendar date based on the hours per week you actually have for this
   project. The gap between the two numbers is your value; the date is your promise.
2. **Feature list to SOW.** A client list goes in. Claude verifies every item against the
   live system first (repo, CRM, ad account), sorts items into already built / partly built
   / net new / blocked, finds what actually drives the cost, packages the work into monthly
   batches, and writes two documents: an internal one with hours and risks, and a
   client-facing one with deliverables and no hours.
3. **An actuals log that recalibrates.** When a job closes, Claude logs estimate vs actual
   and what the estimate missed. After 3 logged jobs of one type, your own median replaces
   the built-in guess. After 5, it uses your real P50 and P80.

Plus a prompt template (ROLE / CONTEXT / TASK / CONSTRAINTS / OUTPUT) for briefing Claude
and for AI steps inside automations.

## Worked example (made-up project)

A dog-grooming salon wants an online booking page, with bookings synced into their CRM and
a confirmation SMS. You ask Claude: "How long will this take?"

Claude decomposes it (booking page, form-to-CRM sync, one CRM workflow) and sums three
buckets: about 30 human-expert hours of generated work divided by 6 = 5h, 3h of manual CRM
setup at human speed, 4h of testing and client review. That's 12h, times 1.5 for the scope
every estimate misses = 18h.

```
TASK: Booking page + form-to-CRM sync + confirmation SMS workflow
Market quote:   50-120 billable hrs (about 2-5 weeks calendar)
You, hands-on:  18 real hrs  [range: 9 - 45, medium confidence, no logged actuals yet]
You, calendar:  18 hrs / 10 hrs per week + 1 week waiting on salon copy and CRM access -> about 3 weeks
Value gap:      about 5x on hours
Confidence:     medium
Biggest uncertainty: whether the salon's CRM plan allows the SMS step
Bottleneck:     CRM access from the client
```

You quote the client the market-based price and the 3-week date. You plan your week on the
18 hours. When it ships, Claude logs that it took 23 hours and that SMS sender registration
was the thing nobody estimated. The next booking-page estimate starts from that.

## Install

You need [Claude Code](https://docs.claude.com/en/docs/claude-code).

macOS / Linux:

```bash
git clone https://github.com/milansolutionsai-ops/claude-code-scoping-kit.git
mkdir -p ~/.claude/skills
cp -r claude-code-scoping-kit/skills/* ~/.claude/skills/
```

Windows (PowerShell):

```powershell
git clone https://github.com/milansolutionsai-ops/claude-code-scoping-kit.git
New-Item -ItemType Directory -Force "$HOME\.claude\skills"
Copy-Item -Recurse claude-code-scoping-kit\skills\* "$HOME\.claude\skills\"
```

Then open `~/.claude/skills/time-estimation/config.md` and set your hourly rate and
currency. Until you do, estimates stay in hours and never show a money figure. Also set
how many hours a week you usually give one project (default 10).

To use it, ask Claude things like "how long will this take?", "scope this feature list",
or "put together a SOW". The skills load on their own. You can also call them directly with
`/time-estimation` or `/scoping`.

Delete the two example rows in `calibration-log.md` once you log your first real job.

## What's in the repo

```
skills/time-estimation/
  SKILL.md             the dual-track method, reference table, confidence ranges
  config.md            your rate, currency, hours per week, divisors
  calibration-log.md   estimate vs actual log (two fake example rows)
skills/scoping/
  SKILL.md             verify, classify, find the cost driver, package, two artifacts
  templates.md         skeletons for the internal and client-facing docs
skills/prompt-template/
  SKILL.md             prompt anatomy, tiers, rules for automations and Claude Code
```

The reference table's AI-assisted column is the author's prior from his own builds, not a
benchmark. Your log is meant to overwrite it.

## License

MIT. Use it, change it, ship it.

Built by [Milan Veljkovikj](https://github.com/milansolutionsai-ops).
