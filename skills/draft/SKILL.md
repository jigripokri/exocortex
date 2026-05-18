---
name: draft
description: Drafts outbound messages (WhatsApp, SMS, email) in the user's voice. Studies recent messages in a thread to mirror cadence. Never sends without explicit approval.
---

# /draft

Write outbound messages in the user's voice. Used on demand for any thread.

## Recipe

When invoked:

1. **Identify the thread.** User says "draft a reply to <person>" or "draft a message to <group> about <topic>." If ambiguous, ask one clarifying question.

2. **Read the last 10–20 messages in the thread.** Use the appropriate MCP (WhatsApp, Gmail, Android Messages).

3. **Study voice patterns.**
   - Sentence length and structure
   - Vocabulary level (formal vs. casual)
   - Regional slang or in-group references
   - Punctuation habits
   - Use of emoji or not
   - Greeting/sign-off patterns (or lack thereof)

4. **Read CLAUDE.md > Voice & Drafting Authorization.**

5. **Read memory/people.md** for context on the recipient (if available).

6. **NEVER read memory/private/carry-on.md.** Do not reference anything from it.

7. **Draft 1–3 options.** If user said "draft" (singular), give one. If user said "options" or the message is high-stakes, give 2–3 with brief notes on the difference.

8. **Show the draft(s).** Do NOT send.

9. **Wait for approval.** "Send it", "send A", "send the second one" = green light. Anything else means iterate.

10. **On approval, send** via the appropriate MCP.

## Voice rules (default — override in CLAUDE.md)

- Direct. No "Hope this finds you well."
- Match the recipient's tone — if they're casual, you're casual.
- Reference something specific from the recent conversation (callback, shared joke, thing they mentioned).
- Short. Two paragraphs maximum unless the topic genuinely requires more.
- No AI tells: "As an AI...", "I hope this helps...", "Let me know if you have any questions!" — never.

## What this skill won't do

- Send before "send it" approval. Always wait.
- Quote anything from carry-on.md.
- Send to multiple recipients without confirming the list explicitly.
- Send if the draft contradicts something obvious from memory (e.g., agreeing to a date the user has a conflict on).
