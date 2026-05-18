---
name: reply
description: Generate 3 reply options + a recommendation for any incoming message thread. The user picks; /draft sends.
---

# /reply

User shows you a thread. You return three plausible replies and a recommendation. They pick.

## Recipe

1. **Read the thread.** Last 10–20 messages.

2. **Identify the actual question or ask.** Strip pleasantries. What does this message actually want from the user?

3. **Read memory/people.md and memory/commitments.md** for relevant context (e.g., have they already promised something here?).

4. **Draft three options** that span the strategy space:

   - **Option A — Direct yes/agreement**: shortest path forward.
   - **Option B — Push back or qualify**: where the user has constraints, hesitation, or a different read.
   - **Option C — Buy time or redirect**: defer, ask a clarifying question, change the venue.

5. **Recommend one** based on:
   - User's stated priorities in life.md
   - The relationship's pattern (from people.md)
   - Open commitments that might be affected

6. **Format the output** as:

```
**They said:** <one-line restatement of the ask>

**Option A — <flavor>**
<draft text>

**Option B — <flavor>**
<draft text>

**Option C — <flavor>**
<draft text>

**Recommendation: <A/B/C>.** <One sentence why.>

**Wildcard:** <one contrarian take they might not have considered>
```

7. **Wait for user to pick.** "Send A" → invoke /draft to send Option A.

## Voice rules

Match the recipient's tone. See `/draft` for full voice rules.

## When NOT to give 3 options

If the message clearly has only one reasonable response (e.g., a simple yes/no logistical question), just give the one reply and a note about why options aren't useful here.
