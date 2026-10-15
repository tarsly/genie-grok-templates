---
name: comm-imessage-fast
description: >-
  Send, read, and search iMessages directly from Claude using the bundled MCP server. Native path — no BlueBubbles, no server process, no ports. Use this any time the user says 'text X', 'iMessage X', 'send an iMessage', 'message the group', or asks 'what did X text me?' / 'what's new in iMessage?'. Prefer this over comm-imessage (BlueBubbles).
---

# COMM-iMessage (Fast)
## Native iMessage via bundled MCP server

---

## Overview

This skill sends, reads, and searches iMessages using the `imessage-fast` MCP server bundled with the communications plugin. It reads `~/Library/Messages/chat.db` directly and sends via AppleScript to `Messages.app`. No BlueBubbles, no localhost server, no passwords.

**Prefer this skill over `comm-imessage`** — that one is the legacy BlueBubbles path and is kept only for existing users.

---

## Prerequisites

Three things must be true on the user's Mac. If any check fails, **invoke the `comm-imessage-fast-setup` skill** to handle it — do not walk the user through the setup manually.

| Requirement | Verify |
|---|---|
| The `imessage-fast` MCP server is running | Call `imessage_search_chats` with a harmless query like `.` — if the tool isn't listed or errors with `Node.js 24+` / `MCP server not connected`, invoke `comm-imessage-fast-setup`. The server is a self-contained bundle — there are no dependencies to install |
| Full Disk Access (read tools only) | If a READ tool errors with "Cannot open ~/Library/Messages/chat.db", relay the error's FDA fix steps — note that SENDING still works without FDA |
| Messages.app open + signed in | If `imessage_send` errors mention AppleScript failure, prompt the user to open Messages.app and sign in with their iMessage Apple ID |

**Recovery rule:** whenever an iMessage tool returns an error mentioning dependencies, Node, MCP server, Full Disk Access, or `chat.db` access, hand off to `comm-imessage-fast-setup` and let it drive the fix. Don't retry the failing tool call until setup confirms tools are ready.

---

## Available MCP Tools

The `imessage-fast` MCP server exposes nine tools. Use them in this order for the common cases below.

| Tool | Purpose | Notes |
|---|---|---|
| `imessage_search_chats` | Find a chat by display name, GUID, or participant handle | Use when the user names a recipient by name and you need the phone/email/chatGuid |
| `imessage_find_group` | Find existing chats containing ALL given participants (2+) | Always call this first for multi-recipient sends — exact matches beat creating anything. Also reports whether the group-send shortcut is installed |
| `imessage_send_group` | Send to multiple people as one group thread | Uses an existing exact-match thread, else creates the group via the "Genie Group Send" shortcut. `fallbackToIndividual` only with explicit user consent |
| `imessage_send_individual` | Send the same message to multiple people as separate 1:1s | Explicitly NOT a group thread. Reports per-recipient success/failure |
| `imessage_read_chat` | Read the most recent messages in a specific chat | Returns both sent and received |
| `imessage_poll_new` | Return new inbound messages since the persisted marker, then advance the marker | Use for "what's new?" — the marker file at `~/Library/Application Support/comm-imessage-fast/state.json` persists across sessions |
| `imessage_peek_new` | Same as `poll_new` but does not advance the marker | Use for inspection without consuming |
| `imessage_send` | Send a message. Requires exactly one of `recipient` (phone/email) or `chatGuid` | Never call this without the confirmation gate below |
| `imessage_reset_marker` | Set the poll marker to a specific rowid or the current max | Recovery only — do not call in normal flow |

---

## Pre-Flight — Preferences

At the start of the skill:
1. Search Cloud Brain for saved preferences: `search_notes` with query `comm preferences`.
2. If found: read the user's name, saved contact shortcuts, and the `## Group Threads` table — a saved group GUID skips the whole find/disambiguate flow.
3. If not found: continue silently — don't block on preferences for a one-off text.

---

## Common Flows

### "Text Mary that I'm running late"

1. Check Cloud Brain preferences for a saved shortcut named "Mary". If present, use it.
2. Otherwise call `imessage_search_chats` with `query: "Mary"` and `limit: 5`.
3. If multiple chats match, list them and ask the user to pick one.
4. If exactly one match: proceed to the confirmation gate.
5. Confirmation gate (mandatory — see safety rule below):
   ```
   READY TO SEND:
     To:      Mary (chat GUID: iMessage;-;+1XXX...)
     Message: "Running late — be there by 4:15"

   Send this message? (yes / edit / cancel)
   ```
6. On explicit "yes" only: call `imessage_send` with either `chatGuid` (for group chats or when you already have it) or `recipient` (E.164 phone or email).
7. Confirm delivery: "✅ Sent to Mary."

### "What's new from Mary?"

1. Call `imessage_search_chats` with `query: "Mary"` to get the chat GUID(s).
2. Call `imessage_read_chat` with the GUID and `limit: 10`.
3. Filter for inbound (`isFromMe: false`) if the user only wants replies to them.
4. Summarize the messages.

### "What's new in iMessage?"

1. Call `imessage_poll_new` with `limit: 50`.
2. Group results by `chatGuid`, summarize per-chat.
3. If the result includes `bootstrapped: true`, tell the user: "This was the first check — I've set the marker to the current max, so next time I'll only return messages that arrive after now."
4. Note: the marker is advanced by this call. If the user wants to see the same messages again without consuming, use `imessage_peek_new` instead.

### "Text the Owner's Club group about tomorrow's meetup"

1. Group chats need the `chatGuid` — call `imessage_search_chats` with a name substring.
2. From the search results, group chats will typically have a non-null `display_name`. Ask the user to confirm the right group if multiple match.
3. Once confirmed, use `imessage_send` with `chatGuid` set to the group's GUID.

### "Text Emma and Tate that dinner moved to 7" (multiple recipients)

**Never assume group vs individual — always ask.** The same two numbers can mean "one group thread" or "two private texts," and they are very different sends.

1. Resolve names to numbers/emails (Cloud Brain shortcuts, then `imessage_search_chats`).
2. Call `imessage_find_group` with all participants. Note whether an exact-match thread exists and whether `groupShortcutInstalled` is true.
   - **If the user includes themselves** ("text me and Luke"), pass their handle anyway — the server detects and strips the user's own numbers/emails (`selfHandlesExcluded` in the result), because macOS never lists the sender as a chat participant. Matching then runs on the remaining people, so "me + Luke" correctly reuses the existing Luke thread. Tell the user they'll see the message as sent in that thread rather than receiving a separate copy.
3. Ask the disambiguation question (mandatory, ONE message):
   > "Send this as a **group thread** (everyone sees each other and the replies) or **individually** (separate private 1:1 texts)?"
   - If an exact-match thread exists, say so: "You already have a group thread with exactly these people — I'd reuse it."
   - If no thread exists and the shortcut is NOT installed, say group requires a one-time setup (`comm-imessage-fast-setup`) or they can pick individual.
4. If they chose **group** and a new thread would need to be created, also ask: "If the group send fails, want me to fall back to sending individually, or stop and tell you?"
5. Confirmation gate (mandatory — extends the standard rule):
   ```
   READY TO SEND:
     To:       Emma (+1801...), Tate (+1801...)
     Mode:     GROUP thread (existing: "…" / new via shortcut)  |  INDIVIDUAL 1:1s
     Failover: fall back to individual sends  |  stop on failure
     Message:  "Dinner moved to 7"

   Send this message? (yes / edit / cancel)
   ```
6. On "yes": `imessage_send_group` (set `fallbackToIndividual` only if authorized in step 4) or `imessage_send_individual`.
7. Report the `method` AND `delivery` from the result honestly: existing thread reused, new group created, delivery `retried-via-applescript` (the shortcut's send was blocked — usually the one-time macOS permission prompt; tell the user to click **Always Allow** in Shortcuts), or individual fan-out — including any per-recipient failures.
8. Save any new group `chatGuid` to Cloud Brain per the Memory section below — don't wait to be asked.

---

## ⚠️ Mandatory Safety Rule — New Contact Confirmation

Any recipient not in the user's saved Cloud Brain shortcuts MUST be confirmed before sending. Show the `READY TO SEND` block, wait for an explicit "yes" or "send it", and only then call `imessage_send`.

**This rule exists because messages send from the user's real iMessage. A wrong send cannot be unsent. Treat every send to a new contact like handing them the phone.**

For saved contacts (present in the Cloud Brain preferences shortcut list), send without an extra confirmation prompt — but still show what's being sent immediately before the send.

If the message contains anything sensitive (legal, financial, personal, apologetic, or angry), flag it:
> "⚠️ This message touches on [sensitive topic]. Double-check before I send."

---

## Job Inputs

At the start of each send flow:
- Who to send to? (contact name, phone number, email, or group name)
- What's the message? (paste it, or describe what you want to say and I'll draft)
- Tone adjustment? (send as-is / more casual / more formal / shorter)

Do not save these to Cloud Brain.

---

## Batch Messaging

To send a similar message to multiple recipients:
1. Draft the base message.
2. List every recipient with their per-recipient personalization if requested.
3. Show all of them at once for approval before sending any.
4. Send only after "send all" or explicit per-recipient approval.
5. If the message is identical for everyone, use `imessage_send_individual` (one call, per-recipient results). If personalized per recipient, loop `imessage_send`.

---

## Memory — Save Contacts and Thread GUIDs

**Contacts** — after a successful first send to a new contact, offer to save them:

> "Want me to save [Name] as a shortcut so I don't need to look them up next time?"

If yes, append to the contacts section of `brain/preferences/comm-preferences.md`. Use the plugin's memory conventions — do not include a `brain/` prefix in the `folder` param.

Store: display name, phone or email. Never store message content.

**Group thread GUIDs** — save these **proactively** (no need to ask) whenever one surfaces: a new group created by `imessage_send_group`, an existing thread matched by `imessage_find_group` that the user confirms is "the" group, or a group the user sends to by name. GUID lookups from Cloud Brain are instant and skip the whole find/disambiguate dance next time.

Maintain a `## Group Threads` section in `comm-preferences`:

```markdown
## Group Threads

| Name | chatGuid | Participants | Last used |
|---|---|---|---|
| Emma + Tate | iMessage;+;chat1234... | Emma (+1801...), Tate (+1801...) | 2026-07-06 |
```

- One row per thread; update `Last used` on reuse rather than appending duplicates.
- Name it what the user calls it ("the family group", "Genie leadership"), not the GUID.
- On any multi-recipient request, check this table FIRST in pre-flight — before `imessage_find_group`.
- If a saved GUID errors on send (thread deleted), remove the row and fall back to `imessage_find_group`.

---

## Error Handling

| Symptom | Meaning | What to say to the user |
|---|---|---|
| Server not present in the MCP tool list | Claude hasn't been restarted since plugin install/update, or Node is missing/too old | "Restart Claude fully (Cmd-Q). If it persists, run the environment check at `<plugin_root>/mcp-server/setup.sh` — it verifies Node 24+, the server bundle, FDA, and the group-send shortcut." |
| Read tool errors mention "Full Disk Access" | The app running Claude lacks FDA (terminal app for CLI, Claude.app for Desktop/Cowork) | Surface the exact fix from the error. Sends still work in the meantime |
| Error mentions "Node.js 24+" | Node too old for the bundled `node:sqlite` server | "Upgrade Node: `brew upgrade node` (or nodejs.org LTS), then restart Claude." |
| `imessage_send` errors on AppleScript | Messages.app not open, or user isn't signed in to iMessage | "Open Messages.app and sign in with your iMessage Apple ID, then try again." |
| `imessage_search_chats` returns empty | Recipient not in chat.db (never texted them from this Mac) | "I can't find a chat with them here. Do you want to send to a specific phone or email?" — if so, use `recipient` in `imessage_send` |
| `imessage_send` succeeds but user says message never arrived | AppleScript said sent, but iMessage delivery pending. Sometimes shows as SMS if recipient isn't on iMessage. | "Messages says it went out. If they don't respond, it might have gone as SMS — worth a follow-up." |
| `imessage_send_group` errors: shortcut not installed | Group creation needs the one-time "Genie Group Send" shortcut | "Creating new group threads needs a one-time shortcut install — run `comm-imessage-fast-setup`, or I can send individually instead." |
| `imessage_send_group` errors: shortcut failed | Shortcuts run failed (permissions prompt, Messages signed out, malformed recipients) | Relay the error. Offer: retry, individual sends, or start the group manually in Messages once (then it's reusable forever). |
| Group send succeeded but no `chatGuid` returned | New thread hasn't landed in chat.db yet | "The message went out — I'll pick up the thread ID next time. Check Messages to confirm it arrived as a group." |

---

## Non-Goals

This skill does **not**:
- Poll continuously for new messages. `imessage_poll_new` is on-demand.
- React autonomously to inbound messages. That's the murph agent's job (see `packages/channels/imessage/` in the ai-agent-unity repo), not a Claude Code skill.
- Handle SMS-only recipients differently. AppleScript send routes through Messages.app which decides iMessage vs SMS.
- Delete or edit sent messages. macOS restricts this and it isn't in scope.
