# Morning Brief

**Schedule:** `0 5 * * *` (5am daily)
**Output:** Email to `{{USER_EMAIL}}` with subject `🌅 Morning Brief — {{date}}`

The big one. Full sweep of all inputs. Lands before the alarm.

---

## Prompt

```
You are running the daily morning brief for {{USER_NAME}}. Follow this recipe exactly.

## Step 0 — Acknowledgment scan

Search Gmail for emails with subject starting `EXO_DONE` received in the last 36 hours.
For each match:
- Extract the slug after `EXO_DONE::`
- Open memory/commitments.md
- Find the matching commitment (slug match, then fuzzy match on title)
- Update its status to DONE with today's date
- Archive the signal email
- If no match found, note it for the email's "Loose Ends" section

## Step 0.5 — Data source health check

Ping each data source in parallel with a minimal read. Record UP/DOWN.
Sources: Gmail, Calendar, Tasks, WhatsApp, Android Messages (if connected), Fitbit (if connected), Monarch (if connected).
If any source is DOWN, surface a `⚠️ DEGRADED: <source>` banner at the top of the email.
NEVER block the briefing on a degraded source — proceed with what's available.

## Step 1 — Memory load

Read in this order:
1. memory/people.md (relationship context)
2. memory/commitments.md (open promises)
3. memory/life.md (priorities + recurring calendar)
4. memory/insights.md (lenses — don't surface, but apply)

DO NOT read memory/private/carry-on.md.

## Step 2 — Calendar scan

Read Google Calendar for the next 24 hours.
Flag: anything before 9am, anything stacked (back-to-back without buffer), anything that conflicts with a recurring block in life.md.
If a "tracked project" event is scheduled, surface it explicitly.

## Step 3 — Inbox scan (Gmail)

Search Gmail for: unread emails in the last 24h, prioritized by tier.
Apply tier weights from CLAUDE.md.
Bucket into:
- 🔥 NEEDS REPLY TODAY (urgent + from someone in tier 1-2)
- 📥 NEEDS REPLY THIS WEEK
- ⏳ FYI (don't surface unless interesting)

## Step 4 — WhatsApp group sweep

For each priority group in CLAUDE.md > Priority WhatsApp Groups, call list_messages with limit 30, parallelize.
Filter for:
- @ mentions of {{USER_NAME}}
- Direct questions ("anyone want to...", "who's coming...")
- Plan-making language ("Saturday", "tomorrow", "let's", "RSVPs")
- Time-sensitive commitments

Bucket as ON YOU (you need to act) vs FYI.

## Step 5 — DM sweep (active chat discovery)

Run search_messages with 6 broad keywords in parallel: "ok", "thanks", "tomorrow", "haha", "please", "yes".
Each with limit 20.
Extract unique chat_jid values from messages in the last 24-48h.
Filter to DMs only (exclude @g.us group JIDs and known bot chats).
For each discovered active DM, call list_messages with limit 10.
Apply commitment detection heuristics (see CLAUDE.md > DM Commitment Sweep).
Detect: unanswered messages to you, confirmed plans, promises you made.

## Step 6 — SMS sweep (if Android Messages connected)

Via Chrome navigate to messages.google.com. Read or screenshot inbox.
Scan any SMS threads with activity in last 24h.
Primary targets: people listed in CLAUDE.md as SMS-primary (often spouse/sibling).

## Step 7 — Tasks check (via Chrome)

Navigate to tasks.google.com.
For each task list flagged "primary" in CLAUDE.md, read open tasks.
Surface anything overdue or due today.

## Step 8 — Health (if Fitbit connected)

Pull yesterday's: sleep duration + score, resting HR, steps, active zone minutes.
If sleep < 6h or HR is anomalous, flag it. Otherwise one-line summary.

## Step 9 — Compose the email

Format:

Subject: 🌅 Morning Brief — <day, date>

Body sections, in this order:
1. ⚠️ Degraded banner (if any)
2. 🎯 Today's three (the three things that ACTUALLY matter today — your call, based on everything above)
3. 🔴 #1 Tracked project update (from CLAUDE.md)
4. 📞 People to reach out to (with reason + tier)
5. 📥 Inbox (NEEDS REPLY TODAY items)
6. 💬 Group chat (ON YOU items only)
7. 💌 DMs (unanswered + commitments detected)
8. 📅 Calendar today (events with prep notes)
9. ✅ Open commitments aging (>3 days)
10. 🧠 Insight (one wildcard observation from the data — see CLAUDE.md > Wildcard Insight)

For EVERY action item, include a `mailto:` acknowledgment link:
`<a href="mailto:{{USER_EMAIL}}?subject=EXO_DONE%3A%3A<slug>">✅ I did this</a>`

Slug rules: lowercase, hyphens, max 40 chars, derived from item title.

## Step 10 — Update memory

For each new commitment detected, add to commitments.md with appropriate status.
For each insight worth keeping, add to insights.md with today's date.
For each person who surfaced with new context, update people.md.
For each commitment marked DONE in Step 0, move it to the Done section.

## Step 11 — Send

Send the email from {{USER_EMAIL}} to {{USER_EMAIL}} (self).

Done. Confirm completion with: "Morning brief sent. <N> action items. <M> commitments updated."
```

---

## Customization notes

- **Length budget:** if the email runs longer than 1200 words, you're surfacing noise. Tighten the filters in Steps 3–5.
- **Wake-up time:** schedule should be 60–90 min before your alarm.
- **Tier weights:** edit CLAUDE.md > Priority Tiers, not this prompt.
- **The "🎯 Today's three" section is the most important.** Three things, ranked. Spend the most reasoning budget here.
