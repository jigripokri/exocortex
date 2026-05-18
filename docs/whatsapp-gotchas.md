# WhatsApp Gotchas

Hard-won learnings from running WhatsApp at scale through the MCP bridge. If you connect WhatsApp, read this.

---

## 1. `list_chats` is broken (don't use it for discovery)

What it should do: return all your chats, sorted by recency, with names and timestamps.

What it actually does: returns LID-based contacts with no names and no timestamps. Useless for discovery.

What to do instead:

- To find a specific group or person: `search_contacts(query="name")`
- To discover active chats: `search_messages` with broad keywords (see #4 below)
- To list messages in a known chat: `list_messages(chat_jid=...)` works fine

---

## 2. `limit` and `page` must be numbers, not strings

This will fail silently:

```python
list_messages(chat_jid=..., limit="10")
```

This works:

```python
list_messages(chat_jid=..., limit=10)
```

Trips up newcomers every time.

---

## 3. `sender_display` is often "Unknown" in groups

WhatsApp's MCP doesn't always resolve sender names in groups. Don't trust the sender field — work with message content and `is_from_me` to attribute.

Use `is_from_me=True` to detect your own messages (essential for finding unanswered threads).

---

## 4. Always parallelize

Scanning 10+ groups sequentially takes 60+ seconds. Parallelize:

```
For all priority groups in CLAUDE.md, fire list_messages calls in parallel.
Same for DM contacts.
```

Modern Claude tool-use supports parallel calls. Use them.

---

## 5. The LID migration (the big one)

**What happened**: Around late March 2026, WhatsApp migrated DM identifiers from phone-based JIDs (`+1XXXXXXXXXX@s.whatsapp.net`) to anonymous LID-based JIDs (`XXXXXXXXXXXXXXX@lid`). The exact timing varied per user and per contact.

**Why it matters**: If you have hardcoded JIDs from before the migration (or even from a few months ago), they may now be partially or fully obsolete. New DM messages get stored under LID format. Old messages may still be queryable under the phone JID.

**The symptom**: You scan a contact's JID. The MCP returns "no messages in last 24h." But you know they texted you yesterday.

**The fix**: For every DM contact you care about, store **both** the phone JID and the LID JID. Scan both. The exocortex's DM sweep recipe (in CLAUDE.md) does this.

### How to discover LID mappings

1. Run `search_messages(query="<person-specific keyword>", limit=20)`. Use something only they'd say.
2. Look at the `chat_jid` values returned. The new LID JID will be in there.
3. Record the mapping in your contacts table.

Repeat for each Tier 1–2 contact.

This is tedious but only has to be done once per contact.

---

## 6. Group messages used to be silently dropped (now fixed)

In early 2026, the MCP bridge had a config bug where group messages were filtered out as if they were status broadcasts. Symptom: group chats appeared empty even though you knew they were active.

**Fix applied**: bridge config changed to only ignore `status@broadcast`. After re-linking + history re-sync, groups work fine via MCP.

If you fork this repo and find groups appearing empty:

1. Check the bridge's `shouldIgnoreJid` config.
2. Re-link WhatsApp (Cowork → Connectors → Re-authorize WhatsApp).
3. Wait for full history sync.

---

## 7. Call logs (voice and video) are invisible to the MCP

The WhatsApp MCP only returns messages, not call records. There's no API for "show me my recent calls."

But call logs DO appear inline in WhatsApp Web chat threads. You can:

1. Navigate to `web.whatsapp.com` via Chrome MCP.
2. Open the relevant chat.
3. Read or screenshot to see call entries.

This matters if calling cadence is part of your relationship tracking — e.g., "I should call mom this week, when did I last call her?"

For most users, the simpler **self-report model** is better:

- The morning briefing asks "When did you last call X?" during the relationship section.
- You answer ("Sunday").
- The exocortex updates people.md with that date.
- Future briefings reason from the stored last-contact date.

Less precise but reliable and zero-effort.

---

## 8. Broadcast lists and channels

WhatsApp Channels and Broadcast lists generally don't surface in the MCP. If they're important to you, you'll need to read them manually via WhatsApp Web. Most users skip this.

---

## 9. Media handling

The MCP returns text content of messages. Media (photos, voice notes, documents) appear as placeholder entries like `[Photo]` or `[Voice note]`.

You generally don't need media for personal-AI use cases — the exocortex reasons about messages, not images. If you need media access, use Chrome → WhatsApp Web.

---

## 10. Disappearing messages

If a chat has disappearing messages enabled (e.g., your private parent chat with 24-hour disappearing), only messages currently visible to your WhatsApp client are scannable. Once they disappear from your app, they're gone from the MCP too.

For high-trust groups with disappearing messages, factor this in: the exocortex won't have continuous memory of those conversations. Important commitments from those chats should be transferred to `commitments.md` immediately.

---

## 11. Don't try to spoof your number

The MCP bridge uses your real WhatsApp account. Don't try to attach a different number, run multiple sessions, or do anything weird. WhatsApp's anti-abuse systems will flag and potentially suspend the account.

---

## 12. Rate limits

WhatsApp has un-documented per-account rate limits. If you scan aggressively (e.g., every 5 minutes against 100 groups), you may hit them.

The default scheduled briefings (morning + evening + weekly) don't come close to hitting limits. Don't go crazier than that without testing.

---

## Summary checklist

When connecting WhatsApp:

- [ ] Wait for full history sync (10–30 min) before running briefings
- [ ] Use `search_contacts` to find groups/people, never `list_chats`
- [ ] Parallelize all reads
- [ ] Store both phone JID and LID JID for important DM contacts
- [ ] Pass numeric `limit` and `page` params
- [ ] Use `is_from_me` to detect unanswered messages
- [ ] Use the self-report model for call-cadence tracking
- [ ] Don't expect group messages to have resolved sender names — work with content
