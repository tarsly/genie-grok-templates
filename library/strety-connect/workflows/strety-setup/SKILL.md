---
name: strety-setup
description: >-
  One-time Strety API connection setup. Walks through creating an OAuth app in Strety (My Integrations > My Apps), captures client credentials, runs the authorization-code flow, stores tokens locally, and verifies the connection. Use when the user says 'connect Strety', 'set up Strety', 'Strety auth', 'my Strety token expired', or any strety-* skill reports missing/invalid credentials.
---

# Strety Setup — Connect Your Strety Account

## Overview

Connects Claude to the Strety API (`https://2.strety.com/api/v1`) via OAuth 2.0 authorization-code flow. Credentials live in `~/.strety/credentials.json` (chmod 600, never in Cloud Brain). Non-secret preferences (default team, your person id) go to Cloud Brain so other strety-* skills skip re-asking.

Strety access tokens expire after 2 hours; the shared helper (`${CLAUDE_PLUGIN_ROOT}/scripts/strety.sh`) auto-refreshes them. This skill is only needed once — or again if the refresh token itself expires or scopes change.

## When This Skill Applies

- User says "connect Strety", "set up Strety", "link my Strety account"
- Any strety-* skill fails with "no credentials" or a refresh failure
- User wants to change OAuth scopes (scope changes require full re-authorization — refresh tokens inherit the original scope)
- User invokes `/strety-setup`

## Pre-Flight — Preferences

1. Check `~/.strety/credentials.json` exists — if yes and `--reauth` not passed, run `bash ${CLAUDE_PLUGIN_ROOT}/scripts/strety.sh WHOAMI` and report the connection is already live.
2. `search_notes "strety preferences"` — existing defaults (team, person id).

**Banner:**
```
🔗 Strety Setup | Status: {connected/not connected} | Scopes: read+write
```

## How It Works

### Step 1: Create the OAuth App (user does this in Strety)

Tell the user, in ONE message:

1. Log in at `https://2.strety.com` → **My Integrations** → **My Apps** → create a new app (name it e.g. "Claude").
2. Set Redirect URI to exactly `https://localhost:8688/callback` (https, not http).
3. Check both scopes: **read** and **write**.
4. Paste back the **Client ID** and **Client Secret**.

### Step 2: Authorization

Give the user the authorize URL to open in their browser:
```
https://2.strety.com/api/v1/oauth/authorize?client_id={CLIENT_ID}&redirect_uri=https://localhost:8688/callback&response_type=code&scope=read+write
```

Capture the authorization code one of two ways:

- **Paste-the-code (default — works everywhere, including Cowork and sandboxed VMs):** After clicking Authorize, the browser lands on a connection-error page at `https://localhost:8688/callback?code=XXXX`. That's expected — ask the user to copy the full URL (or just the `code` value) from the address bar and paste it back.
- **Listener (CLI on the same machine only):** Start `nc -l 8688` in the background before the user authorizes, then read the code from the captured request line `GET /callback?code=XXXX HTTP/1.1`. Don't use this in Cowork or any environment where the shell runs in a VM — the browser's localhost is the host machine, not the sandbox, so the redirect never reaches the listener.

### Step 3: Exchange Code for Tokens

```bash
curl -s -X POST "https://2.strety.com/api/v1/oauth/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=authorization_code" \
  -d "code={CODE}" \
  -d "client_id={CLIENT_ID}" \
  -d "client_secret={CLIENT_SECRET}" \
  -d "redirect_uri=https://localhost:8688/callback"
```

### Step 4: Store Credentials

Write `~/.strety/credentials.json` (create `~/.strety/` first, then `chmod 600` the file):

```json
{
  "client_id": "...",
  "client_secret": "...",
  "access_token": "...",
  "refresh_token": "...",
  "saved_at_epoch": 1234567890
}
```

`saved_at_epoch` is `date +%s` at save time — the helper uses it to refresh proactively.

### Step 5: Verify + Save Preferences

1. `bash ${CLAUDE_PLUGIN_ROOT}/scripts/strety.sh WHOAMI` — must return a people list.
2. `bash ${CLAUDE_PLUGIN_ROOT}/scripts/strety.sh GET /teams` — show teams, ask which is the default.
3. Match the user's email in the people list to find their person id.
4. Write preferences note to Cloud Brain (folder `preferences` — never a `brain/` prefix).

## Data Structure

Cloud Brain note `Strety Preferences` in folder `preferences`:

```markdown
# Strety Preferences

- **Default Team (space):** {name} — id: {uuid}
- **My Person ID:** {uuid}
- **My Email:** {email}
- **Scopes:** read, write
- **Connected:** {YYYY-MM-DD}
```

Tags: `["strety", "preferences", "integration"]` (YAML list, not a JSON string).

## Output Format

```
✅ Strety connected
- Account: {email}
- Default team: {team name}
- Scopes: read + write
- Credentials: ~/.strety/credentials.json (local only, chmod 600)

Try: /strety-todos list
```

## Example Usage

> **User:** connect my Strety account
> **Claude:** Walks through app creation, opens auth flow, exchanges the code, verifies with WHOAMI, saves prefs, prints the success banner.

## Error Handling

- **"Not Found" on the authorize URL** — Redirect URI mismatch. It must match the Strety app settings character-for-character, including `https://`.
- **`invalid_grant` on token exchange** — Codes are single-use and short-lived. Re-run the authorize step for a fresh code.
- **Refresh fails later** — Refresh tokens can expire. Re-run `/strety-setup --reauth`.
- **Scope errors on writes** — Refresh tokens inherit the original scope; adding `write` after the fact requires full re-authorization, not a refresh.
- **Port 8688 busy** — Pick another port, but the Redirect URI in the Strety app settings must be updated to match before authorizing.

## See Also

- `/strety-todos`, `/strety-rocks`, `/strety-scorecard`, `/strety-issues` — the day-to-day skills this unlocks
- `/strety-sync` — mirror Strety into Cloud Brain for the `eos-operator` plugin
