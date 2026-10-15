---
name: comm-imessage
description: >-
  Legacy skill — use comm-imessage-fast instead for faster delivery (no BlueBubbles required). This version sends iMessages via the BlueBubbles MCP connector and is kept for compatibility. Use when the user says 'send iMessage via BlueBubbles', 'message [person] on iMessage via BlueBubbles', or needs to send to a contact managed through BlueBubbles.
---

# COMM-iMessage
## Send iMessages via BlueBubbles

> **LEGACY.** New work should use `comm-imessage-fast`, which ships a bundled MCP server that talks directly to `Messages.app` and reads `chat.db` — no BlueBubbles required. This skill stays available for users still running BlueBubbles.

---

## Overview

This skill sends iMessages to individuals or groups via the BlueBubbles MCP connector. Requires the BlueBubbles app running on a Mac with an active iMessage account, and the BlueBubbles MCP configured in Cowork.

**Connector required:** BlueBubbles MCP. If not connected, this skill will guide setup.

**No regulated disclaimer required for this skill.**

---

## Pre-Flight — Connector Check

1. Verify the BlueBubbles MCP connector is active
2. If not active: display setup instructions:
   ```
   To use iMessage from your AI:
   1. Install BlueBubbles on your Mac (bluebubbles.app)
   2. Connect the BlueBubbles MCP in Cowork Settings → Connectors
   3. Return and re-run this skill
   ```
3. Search Cloud Brain: `search_notes` with query "comm preferences" — check for saved contacts/groups
4. If preferences found: confirm sender identity in one line
5. If not found: collect in ONE message:
   - Your name (for message sign-off if needed)
   - Any frequently messaged contacts or groups to save as shortcuts

---

## Job Inputs

Ask at the start of each run:
- Who to send to? (contact name, phone number, or group name)
- What's the message? (paste it or describe what you want to say)
- Tone adjustment needed? (send as-is / make it more casual / make it more formal / shorten it)

Do not save these answers to Cloud Brain.

---

## Processing Logic

1. Look up the recipient in saved contacts/groups from Cloud Brain preferences
2. If not found, ask for the phone number or group handle
3. If the user described a message rather than writing it: draft the message, show it, and ask for confirmation before sending
4. Show the message and recipient for confirmation:
   ```
   READY TO SEND:
   To: [recipient]
   Message: "[message text]"

   Send this message? (yes / edit / cancel)
   ```
5. Only send after explicit user confirmation
6. Confirm delivery: "✅ Message sent to [recipient]"

**Never send a message without showing it to the user first and receiving explicit confirmation.**

---

## Batch Messaging

If the user wants to send a similar message to multiple recipients:
1. Draft the base message
2. List all recipients with their personalized version (if any customization requested)
3. Show all at once for confirmation before sending any
4. Send only after "send all" or selective approval

---

## Memory — Save Contacts

If a new contact is messaged, offer to save them:

**Path:** `brain/preferences/comm-preferences.md` — append to contacts section

Ask: "Want me to save [Name] as a contact so you don't need to look up their number again?"

---

## Error Handling

- **BlueBubbles not connected:** Show setup instructions, do not attempt to send
- **Recipient not found:** Ask for phone number — do not guess
- **Message is sensitive (legal, financial, personal):** Flag it: "⚠️ This message contains sensitive content — double-check before confirming"
- **Group not found:** List available groups from saved preferences and ask which one
- **Delivery failure:** Report the error clearly and suggest checking the BlueBubbles connection
