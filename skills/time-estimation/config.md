# Estimating settings

Claude reads this file before every estimate. Edit the values; leave a value as `SET_ME` and
Claude will ask once or leave that line out of the estimate.

| Setting | Value | Notes |
|---|---|---|
| Hourly rate | SET_ME | Your billable rate. Without it, estimates stay in hours and no money figure is shown. |
| Currency | SET_ME | e.g. USD, EUR, GBP |
| Committed hours per week | 10 | Default hours per week you give ONE project. Used for the calendar date when you don't say. |
| AI speed-up divisor | 6 | Human-expert effort divided by this = your hours for generated work. A placeholder until your log has 3+ actuals for an archetype. |
| Overrun multiplier | 1.5 | Applied to your hands-on track for omitted scope. Tune it from the omissions column in the log. |
