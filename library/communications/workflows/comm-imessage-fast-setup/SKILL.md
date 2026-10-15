---
name: comm-imessage-fast-setup
description: >-
  One-time setup for the comm-imessage-fast MCP server. Use this when the user says 'set up iMessage', 'install iMessage plugin', 'iMessage isn't working', 'why can't Claude send iMessages', or when ANY imessage_* tool errors with server-not-loaded / dependencies-missing / node-not-found. Also invoke proactively the first time the user tries to use comm-imessage-fast and it fails. Validates the environment for the bundled self-contained MCP server (Node 24+, Full Disk Access, group-send shortcut) and verifies the tools come online — nothing to install.
---

# COMM-iMessage-Fast — Setup

## When to run this skill

Invoke this skill in any of these situations:

- User asks: "set up iMessage", "install iMessage", "why isn't Claude texting", "iMessage isn't working", "get iMessage running"
- Any `imessage_*` tool returns an error mentioning `Dependencies not installed`, `Node.js`, `node_modules`, or `MCP server` unavailable
- After a fresh `/plugin install communications@my-business-genie-library` — before the first `imessage_send`
- After the plugin updates and something breaks

This skill is idempotent — safe to re-run.

---

## Steps

### 1. Locate the plugin install

Claude Code and Claude Desktop install plugins in different places. Find the setup script in either location:

```bash
SETUP=$(find \
  ~/.claude \
  ~/Library/Application\ Support/Claude \
  -path '*/communications/mcp-server/setup.sh' \
  2>/dev/null | head -1)
```

If `SETUP` is empty, the plugin isn't installed. Tell the user:

> "I can't find the `communications` plugin. Install it first with:
> ```
> /plugin marketplace add <your-plugin-source>
> /plugin install communications@my-business-genie-library
> ```
> Then re-run this skill."

### 2. Check Node.js is available

The MCP server needs Node LTS ≥ 20.

```bash
node -v 2>/dev/null || echo "NODE_MISSING"
```

If output is `NODE_MISSING` or the major version is below 20, tell the user:

> "Node.js LTS 20+ isn't installed. Install it first:
> ```
> brew install node        # macOS with Homebrew (fastest)
> ```
> Or download the LTS installer from https://nodejs.org and re-run this skill."

Stop and wait for the user to install Node. Don't proceed until `node -v` prints a version ≥ 20.

### 3. Run the environment check

```bash
cd "$(dirname "$SETUP")" && ./setup.sh
```

Expected output ends with `✅ Check complete.` As of communications v2.4.0 the server is a single self-contained bundle — the script only validates, it installs nothing.

If the script errors, capture the error and share it with the user. Common failures:

| Error | Fix |
|---|---|
| `Node.js 24+ required` | `brew upgrade node` or install the LTS from nodejs.org |
| `dist/index.cjs missing` | Reinstall/update the communications plugin (v2.4.0+) |

### 4. Check Full Disk Access

The MCP server reads `~/Library/Messages/chat.db`, which requires Full Disk Access on macOS. Test whether the process that runs the MCP server has it:

```bash
sqlite3 ~/Library/Messages/chat.db "SELECT 1;" 2>&1
```

- If output is `1` — FDA is granted, proceed.
- If output contains `authorization denied` or `unable to open` — FDA is missing.

**Important: FDA must be granted to the right process.**

- **In Claude Code (terminal-based):** FDA must be granted to the terminal application that launches `claude` (Terminal.app, iTerm.app, Warp.app, etc.).
- **In Claude Desktop:** FDA must be granted to `/Applications/Claude.app` itself.

Tell the user:

> "The MCP server needs Full Disk Access to read your Messages database. To grant it:
>
> 1. Open **System Settings → Privacy & Security → Full Disk Access**
> 2. Click `+` and add:
>    - **Claude Desktop users:** `/Applications/Claude.app`
>    - **Claude Code users:** your terminal app (Terminal, iTerm, or Warp)
> 3. Toggle it **on**
> 4. Fully quit Claude (Cmd-Q for Desktop, or close and reopen your terminal for Code)
> 5. Reopen Claude
>
> After granting FDA and restarting Claude, iMessage tools will work automatically."

### 5. Verify the MCP server is up

After the user restarts Claude, verify tools are available. Try:

```
imessage_search_chats(query=".", limit=1)
```

- If it returns a result (or an empty list) — setup succeeded. Confirm: "✅ iMessage is ready. Try 'text Mary that I'm running late' or 'what's new in iMessage?'."
- If it errors — surface the error verbatim. Common causes:
  - `Cannot open ~/Library/Messages/chat.db` → FDA not granted to the process actually running the MCP server. Re-check step 4.
  - `MCP server not connected` → Claude wasn't restarted after `setup.sh`. Ask user to fully quit and reopen Claude.
  - `Node.js 24+ required` → Node version too old. Redo step 2.

### 6. Optional — install the group-send shortcut

Only needed if the user wants to **create new group threads** (sending to an existing group, single recipients, and individual fan-out all work without it). Messages' AppleScript API cannot create group chats; the plugin bridges through an Apple Shortcuts workflow instead.

1. Check whether it's already installed:
   ```bash
   shortcuts list | grep -x "Genie Group Send"
   ```
   If it prints the name, skip this step.
2. Open the signed shortcut that ships with the plugin:
   ```bash
   open "<plugin_root>/mcp-server/shortcuts/Genie Group Send.shortcut"
   ```
   The Shortcuts app opens an import preview — tell the user to click **Add Shortcut**. Do not rename it: the MCP server looks it up by the exact name "Genie Group Send".
3. Verify with `shortcuts list | grep -x "Genie Group Send"` again.
4. First live run may pop a one-time Shortcuts permission prompt ("Allow 'Genie Group Send' to send messages?") — tell the user to click **Always Allow**.

**If the import fails or the shortcut misbehaves**, the user can build it by hand in the Shortcuts app (2 minutes, 5 actions) — name it exactly `Genie Group Send`:

| # | Action | Configuration |
|---|--------|---------------|
| 1 | Split Text | Text: **Shortcut Input** · Separator: Custom · `\|\|\|Genie\|\|\|` |
| 2 | Get Item from List | **Item At Index** 1 of Split Text |
| 3 | Split Text | Text: **Item from List** (step 2) · Separator: Custom · `,` |
| 4 | Get Item from List | **Item At Index** 2 of the FIRST Split Text (step 1) |
| 5 | Send Message | Message: **Item from List** (step 4) · Recipients: **Split Text** (step 3) · "Show When Run" OFF |

Also enable Shortcuts → Settings → Advanced → **Allow Running Scripts**.

---

## What this skill does NOT do

- **It does not install the plugin itself.** The user runs `/plugin install` first; this skill handles the post-install setup.
- **It does not grant FDA.** macOS requires user interaction in System Settings — no CLI equivalent.
- **It does not restart Claude.** The user must Cmd-Q Claude Desktop or close-and-reopen their terminal for Claude Code. Prompt them to do it.

---

## Quick reference — the one-liner

For power users who just want the setup command:

```bash
SETUP=$(find ~/.claude ~/Library/Application\ Support/Claude -path '*/communications/mcp-server/setup.sh' 2>/dev/null | head -1) && [ -n "$SETUP" ] && (cd "$(dirname "$SETUP")" && ./setup.sh)
```

Copy-paste, hit enter, fix anything it flags, restart Claude. Done.
