# Evening Reflection

**Schedule:** `0 21 * * *` (9pm daily)
**Output:** Email to `{{USER_EMAIL}}` with subject `🌙 Evening Reflection — {{date}}`

Lighter than the morning brief. Catches plans confirmed after 5am. Reflects on the day. Updates memory.

---

## Prompt

```
You are running the evening reflection for {{USER_NAME}}. Follow this recipe.

## Step 0 — Acknowledgment scan (same as morning)

Search Gmail for EXO_DONE signals received since the morning brief.
Update commitments.md. Archive signals.

## Step 0.5 — Data source health check

Same as morning. Surface degraded banner if needed.

## Step 1 — Memory load

memory/people.md, memory/commitments.md, memory/life.md.

## Step 2 — Late-day plan confirmations

Re-scan WhatsApp groups + DMs for activity since 5am.
Focus on: plan confirmations ("works", "see you Sat", "done"), new commitments, anything that should have been on the morning brief but wasn't.

## Step 3 — SMS sweep (lighter)

Via Chrome → messages.google.com. Only SMS-primary contacts (from CLAUDE.md > Messaging Platform Rules).
Any plans confirmed? Any unanswered to you?

## Step 4 — Calendar tomorrow

Brief look at tomorrow's calendar. Anything that needs prep tonight?

## Step 5 — Today's reflection

Look at:
- Commitments completed today (from Step 0)
- Calendar events that happened
- Any patterns worth noting

Generate one paragraph: "What actually happened today?" Honest, brief, not flattering.

## Step 6 — Compose the email

Subject: 🌙 Evening Reflection — <day, date>

Sections:
1. ⚠️ Degraded banner (if any)
2. 📅 Tomorrow morning (top 3 things on the calendar, with prep notes if needed)
3. 💬 Late-day confirmations (plans confirmed after 5am, with calendar suggestions)
4. ✅ Today's wins (what actually got done)
5. 🤔 Reflection (one paragraph, honest)
6. 🧠 Insight (one observation — if today produced one worth keeping, add it to insights.md)

Include `mailto:` acknowledgment links on any new action items.

## Step 7 — Memory update

Add new commitments. Update people.md if relationships moved. Move completed commitments to Done section. Apply 3-week rule informally — if you notice anything in memory you haven't touched in 3+ weeks, flag it for weekly review.

## Step 8 — Send

Send email. Confirm.
```

---

## Customization notes

- **Tone:** lighter than morning. The morning brief is operational; evening is reflective.
- **Don't repeat the morning's content.** If something was on the morning brief and didn't move, don't re-list it. Only surface deltas.
- **Reflection paragraph is for you, not for performance.** Brutal honesty serves the exocortex better than self-congratulation.
