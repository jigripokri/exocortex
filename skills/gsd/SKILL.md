---
name: gsd
description: Generate a prioritized punch list from open commitments + tasks, right now. Get Shit Done.
---

# /gsd

User says "/gsd" or "what should I do next?" You return a prioritized punch list.

## Recipe

1. **Read memory/commitments.md.** Filter to PENDING + OVERDUE.

2. **Read Google Tasks** (via Chrome → tasks.google.com). All open items from primary lists.

3. **Read memory/life.md > This Month / This Week.** Anything that should be active.

4. **Prioritize** using the GSD ranking:

   - **P0 — Overdue + blocking someone else**: do today, in the next 2 hours if possible
   - **P1 — Overdue, self-blocked**: do today
   - **P2 — Today's commitments**: scheduled for today, not done
   - **P3 — This week**: due in next 5 days
   - **P4 — Quick wins**: < 10 min items, batch them
   - **P5 — Aging but not urgent**: > 7 days old, no due date — flag for review/punt

5. **Format the output** as:

```
🎯 GSD — <current time>

P0 (do now):
1. <title> — <est time> — <one-line why it matters>
2. ...

P1 (do today):
1. <title> — <est time>
...

P2 (today's scheduled):
...

Quick wins (5 min, batch these):
- <item>
- <item>

To punt or kill: <N> items aging past usefulness — review during weekly?

**Wildcard:** <one observation about the list — pattern, hidden priority, missing item>
```

6. **Suggest a focus block.** "If you only have 90 min today, do P0 + the top 2 of P1."

## What this skill is for

When the user feels overwhelmed and wants to be told what to do. Returns a clear ranked list, not a vague "you have a lot going on."

## What this skill is NOT

- A todo list manager. The exocortex doesn't modify Google Tasks unless asked.
- A motivator. No "you got this!" — direct ranking, that's it.
