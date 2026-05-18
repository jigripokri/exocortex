# Privacy & Safety

What the exocortex sees, where it lives, and how to keep it from leaking.

---

## What lives where

| Thing | Location | Sensitivity |
|-------|----------|-------------|
| `CLAUDE.md` (with your placeholders filled in) | Your machine | Medium — contains personal context |
| `memory/*.md` | Your machine | High — your second brain |
| `memory/private/carry-on.md` | Your machine | Highest — never leaves, never quoted |
| Briefing emails | Your email inbox | High — summarizes everything above |
| Claude API calls | Anthropic's servers (transient) | Treated per Anthropic's policies |
| `briefings/`, `skills/`, `docs/` (this repo's content) | GitHub (if you push) | None — generic prompts |

The only thing that should ever be on GitHub: the public repo template (this content) and your edits to *prompts*. Never your *memory*.

---

## The single most important rule

**`.gitignore` excludes `memory/` by default. Do not change this.**

Verify before your first push:

```bash
cat .gitignore | grep memory
# should output: memory/
```

If you accidentally pushed memory to GitHub (it happens), use `git filter-repo` to scrub history. Don't just delete the file in a new commit — the data is still in history.

---

## What Claude sees

When briefings run, Claude receives:

- The full content of `CLAUDE.md`
- The content of memory files (most of them — `carry-on.md` is gated)
- The data returned by MCP calls (your messages, emails, calendar events, etc.)
- The prompt itself (the briefing's recipe)

Claude's data handling follows [Anthropic's privacy policy](https://www.anthropic.com/legal/privacy). For consumer plans (Pro, Max), data is not used to train models by default. For API direct usage, same default. Check your settings.

---

## The carry-on file

`memory/private/carry-on.md` is the highest-trust file. The protections in place:

1. **CLAUDE.md explicitly forbids automatic reads.** The persona section says: never read this file unless the user explicitly invokes it.
2. **Every briefing prompt explicitly excludes it from Step 1 (Memory load).**
3. **The `/draft` skill explicitly refuses to quote from it.**
4. **The file itself has a banner reminder at the top.**

These are belt + suspenders. They work in practice. But you should still:

- Not put anything in carry-on.md you couldn't tolerate being briefly visible in a single conversation if you accidentally invoked it
- Audit periodically (read carry-on.md occasionally to remember what's there)
- Treat it like a sensitive document, not a vault — a determined adversary with access to your machine could read it

---

## Threat model

What this system protects against:

- ✅ Accidental memory leaks into outbound communication (draft emails, calendar invites, etc.)
- ✅ Casual exposure if someone glances at your laptop screen
- ✅ Acknowledgment signal emails being readable noise in your inbox

What this system does NOT protect against:

- ❌ Physical access to your machine. (Use full-disk encryption.)
- ❌ A compromised Claude account. (Use 2FA.)
- ❌ A compromised email account. (Same.)
- ❌ Malware. (Standard hygiene applies.)
- ❌ Subpoenas or legal process targeting your email provider or Anthropic. (These are stored at your providers under their normal data handling.)

---

## Financial data

`memory/finances.md` may contain:

- Account balances
- Holdings
- Net worth
- Tax info
- Properties

This is high-value data. Two specific safeguards:

1. **Never push to a public repo.** Verify gitignore.
2. **Treat the monthly financial briefing email as sensitive.** It's emailed to yourself by default. If you want to add a partner CC, do it deliberately — and recognize their inbox may have different security posture than yours.

If you're not comfortable with Claude reading your financial accounts, skip the monthly finance briefing entirely. The other six work fine without it.

---

## Health data

`memory/health.md` may contain:

- Bloodwork
- Diagnoses
- Medications
- Mental health notes

In the US, this is HIPAA-adjacent (but not technically HIPAA-protected once it's in your own personal note system).

Recommendations:

- Don't include diagnoses or medications you're not comfortable having in your Claude conversation history.
- For sensitive mental health context, use carry-on.md, not health.md.
- The monthly briefings don't surface health data unless there's a specific reason — but a briefing could mention it incidentally. Review the first few months' outputs.

---

## What to do if memory leaks

Steps in order:

1. **Identify what leaked and where.** (Briefing email? Public commit? Accidental Slack share?)
2. **If a public commit**: scrub git history immediately. `git filter-repo` is the right tool.
3. **If an email**: depends on the recipient. If you sent it to yourself, no action needed; archive it. If you sent it to a partner, talk to them.
4. **Update CLAUDE.md** to add a rule preventing the same class of leak.
5. **Update the specific briefing or skill** that produced the leak.

The exocortex is software you've configured. Every leak is a bug you can fix.

---

## Children and family

If your memory includes context about kids or other family members who haven't consented to being in an AI system:

- They legally can't consent — that's a parent/guardian decision.
- Treat their data with extra care.
- Don't include anything in memory you wouldn't want them to read at age 18.
- Don't quote memory about them in any outbound communication, ever (not even drafts that you might forget to delete).

---

## Audit checklist (monthly)

Once a month, spend 10 minutes:

- [ ] Open `.gitignore`. Confirm `memory/` is excluded.
- [ ] Run `git log -- memory/` — should return nothing committed.
- [ ] Search Gmail for "EXO_DONE" — old signals should be archived/trashed.
- [ ] Skim recent briefing emails — anything in them you wouldn't want read by someone else? If yes, tighten the prompt.
- [ ] Read carry-on.md. Anything stale? Prune.

---

## TL;DR

- `.gitignore` excludes `memory/`. Verify.
- `carry-on.md` is gated. Trust but verify.
- Briefings go to your email. Make sure your email is secure.
- The exocortex is software. Bugs are fixable. Leaks are recoverable.
