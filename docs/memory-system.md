# The Memory System

Why plain markdown beats vector databases for personal AI.

---

## The architecture in one sentence

Seven markdown files. Read at the start of every task. Updated at the end of every briefing. Pruned weekly. Gitignored by default.

That's it. There is no database. There is no vector store. There is no RAG. The agent just reads files.

---

## Why this works (and why fancier doesn't)

When this repo started, the default architecture for "personal AI memory" was:

1. Embed every input as a vector
2. Store in a vector DB
3. Retrieve via similarity search at query time
4. Pass top-K chunks into the context

This sounds great. It's wrong for this problem. Three reasons:

**1. The corpus is too small.**
A real person's memory file system, after a year of use, is ~50KB across seven files. Modern Claude windows hold 200K+ tokens. There is no retrieval problem. Just load the files.

**2. Vector search makes you stupid.**
Vectors retrieve by *semantic similarity*. But memory retrieval should often be *exact* — what's the status of commitment X? When did I last talk to person Y? Vectors are bad at this. Markdown grep is perfect at this.

**3. The state has to be auditable.**
A vector DB is opaque. If the agent gets something wrong about your life, you can't open the DB and fix it. With markdown, you open the file in any editor and rewrite. The exocortex's memory should be inspectable and correctable by you in 30 seconds.

---

## The seven files

### Core four (always loaded)

| File | Purpose | Read frequency |
|------|---------|----------------|
| `people.md` | Relationship context | Every briefing |
| `commitments.md` | Open promises | Every briefing |
| `life.md` | Goals & priorities | Every briefing |
| `insights.md` | Strategic lenses | Every briefing |

### Domain three (loaded on demand)

| File | Purpose | Read frequency |
|------|---------|----------------|
| `finances.md` | Money state | Only when finance question or monthly sweep |
| `health.md` | Health state | Only when health question |
| `<person>.md` *(optional)* | Per-person deep context | When that person is the subject |

### Private (gated)

| File | Purpose | Read frequency |
|------|---------|----------------|
| `private/carry-on.md` | Sensitive context | NEVER automatic. Only when explicitly invoked. |

---

## Read at start, update at end

Every briefing follows this pattern:

```
Step 1: Read memory files (the "load")
Steps 2..N: Do the work (scan inputs, compose output)
Step N+1: Write memory updates (the "save")
```

The "save" is the most important part. If briefings don't write back, the exocortex doesn't actually learn anything.

What gets written:

- **New commitments** → `commitments.md` under "I Said I Would" or "They Said They Would"
- **Completed commitments** → moved to "Done" section
- **New relationship context** → `people.md` updated
- **New insights** → `insights.md` (sparingly — quality > quantity)
- **Pattern changes** → `life.md`

---

## The 3-week rule

Memory grows. If you don't prune, performance degrades. The 3-week rule:

> If an item in memory hasn't been referenced, updated, or relevant in 3 weeks, archive or remove it during weekly review.

The weekly review briefing does this automatically. It flags candidates for archive; you approve via the email's mailto: links.

What this prevents:

- `people.md` growing to include everyone you've ever met
- `commitments.md` filling with stale "I'll get to it eventually" items
- `life.md` listing goals you've quietly given up on

What this enables:

- Memory load (Step 1 of every briefing) stays fast
- Action items in briefings remain high-signal
- You actually trust the exocortex's memory because it's current

---

## What to save vs. not save

**Save it** if it will matter in a week or more:

- Major decisions and their reasoning
- Recurring relationships (people who appear regularly in your life)
- Active deadlines
- Promises you've made or been made
- Non-obvious patterns you've noticed
- Things you'd want to remember but won't if you don't write them down

**Don't save it** if it's:

- A one-off task you've already done
- Social chatter ("had a great call with X")
- Anything you can re-derive by re-scanning WhatsApp/Gmail/Calendar
- Admin noise (every receipt, every confirmation email)

A good gut check: *If I needed this in a year, would I want it preserved?* If no, don't save it.

---

## Memory hygiene tips

**Edit by hand sometimes.**
The exocortex updates memory automatically, but you should occasionally open the files yourself and prune. It catches things the agent missed.

**Use the templates as scaffolding, not as cages.**
The structure in the templates is a starting point. If a section doesn't apply to you, delete it. If you need a new section, add it.

**Don't aim for completeness.**
The 20/80 rule applies. The first 20% of memory captures 80% of the value. Don't spend 3 hours on `people.md` trying to document every relationship. Get the top 15 right.

**Versioning is your friend.**
Even though `memory/` is gitignored by default, consider keeping a *private* git repo of your memory files (separate from this public repo). Version history saves you when you accidentally delete something.

---

## The case for plain text

Pretty much every nice thing about this system comes from "it's just markdown":

- ✅ You can edit it in any editor (VS Code, vim, Notes app, anything)
- ✅ You can grep it
- ✅ You can version-control it
- ✅ You can diff it
- ✅ You can read it without the agent
- ✅ You can take it with you if you leave Claude (everything is in human-readable files)
- ✅ Models keep improving but markdown stays markdown — no migration

Compare to a vector DB:

- ❌ You can't read it
- ❌ You can't edit a single record cleanly
- ❌ Migrating models often means re-embedding everything
- ❌ The state is invisible to you

Plain markdown wins. It's not flashy. It's right.

---

## Failure modes

**Memory contradicts the agent's other inputs.**
e.g. people.md says X lives in Tokyo, but a recent WhatsApp message says they just moved to Berlin. The agent should: notice the contradiction, update memory with the new info, flag the change in the next briefing.

**Memory is overspecified and breaks naturally.**
e.g. you wrote "Mom calls every Sunday at 7pm" but it's been a Saturday morning thing for a month now. The 3-week rule catches this — facts that don't match observed reality get questioned during weekly review.

**Memory contains real errors.**
The agent gets your sibling's name wrong. You correct it once in the file. Done. (Try fixing a vector DB error.)

---

## Extension: per-person memory files

If someone in your life needs ongoing detailed context — a child you're parenting through a school transition, an aging parent with multiple health issues, a high-touch direct report, a co-founder — create `memory/<name>.md` for them.

Structure (suggested):

```markdown
# <Name>

## Profile
Age, location, role/relationship.

## What's current
What's happening with them right now.

## Recurring themes
Patterns you want to remember.

## History
Important past events.

## Open threads
Same format as commitments.md.

## Notes
Anything else.
```

Read this file when they're the subject of conversation. Update after meaningful interactions.

This is the most underrated extension. Most people need ~3 of these. Common picks: a young child going through a school transition, an aging parent with health complexity, a co-founder, a direct report you're coaching.

---

## TL;DR

Plain markdown. Read at start. Write at end. Prune weekly. Don't overthink it.
