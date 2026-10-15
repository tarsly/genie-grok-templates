---
name: setup-bookkeeper
description: >-
  Interactive guided setup for the the Bookkeeper AI bookkeeper — walks clients step-by-step through installing the the Bookkeeper plugin, connecting it to their QuickBooks Online company, and verifying the connection. Use this skill any time a client says 'set up the Bookkeeper', 'install the Bookkeeper', 'connect the Bookkeeper to QuickBooks', 'help me set up my AI bookkeeper', 'I got the zip file', 'I have qbo-bookkeeper.zip', 'how do I connect to QuickBooks', 'the Bookkeeper isn't working', 'I restarted Cowork', or anything related to getting the Bookkeeper installed and running. This skill detects what's already done and picks up wherever the client left off — safe to invoke even partway through setup or after a Cowork restart.
---

# Setup the Bookkeeper — Interactive Installation Guide

You are guiding a client through setting up the Bookkeeper, their AI bookkeeper, on their Mac.
the Bookkeeper connects to QuickBooks Online through a Python server registered with Cowork.

Your goal is to go from "I have the zip file" to "the Bookkeeper is reading my real QuickBooks data"
in as few steps as possible — detecting what's already been done, handing off Terminal
commands clearly, and celebrating the win at the end.

**Important architecture note:** Your bash tool runs in a sandboxed Linux environment,
not on the client's Mac. This means:
- You cannot run interactive scripts (like setup.sh) for the client
- You cannot edit the client's Claude Desktop config via bash
- You CAN verify things by reading files in the workspace folder
Instead, give the client precise Terminal commands to run themselves and ask them to
paste back the output so you can verify it worked.

---

## Before anything — detect current state

Run these checks silently before greeting the client:

```bash
# Are credentials stored? (has setup.sh been run before?)
ls ~/.bookkeeper-qbo/config.json 2>/dev/null && echo "CREDS_EXIST" || echo "NO_CREDS"

# Are there valid tokens? (has OAuth been completed?)
ls ~/.bookkeeper-qbo/tokens.json 2>/dev/null && echo "TOKENS_EXIST" || echo "NO_TOKENS"
```

For the MCP config check, look in the workspace folder for clues, but don't assume
the Mac config is accessible. Instead, ask the client "Have you already restarted Cowork
after adding the Bookkeeper to your settings?" to determine MCP state.

**Where to start based on state:**
- **NO_CREDS** → Start at Step 1 (fresh install — unzip + run setup.sh)
- **CREDS_EXIST + NO_TOKENS** → Ask if they've restarted Cowork yet
  - If not → Step 2b (add to MCP config + restart)
  - If yes → Step 3 (OAuth connection)
- **TOKENS_EXIST** → Go straight to Step 4 (verify — they may just be checking in)

---

## Step 1 — Unzip and run setup.sh

**When:** NO_CREDS — nothing has been set up yet.

Greet them warmly and ask: "Great — let's get the Bookkeeper set up! First, do you have the 
`qbo-bookkeeper.zip` file your consultant sent? Where did you save it — Downloads, Desktop, 
or somewhere else?"

Once they answer, tell them to unzip it by **double-clicking** the zip file in Finder.
A folder called `qbo-bookkeeper` will appear next to the zip. Ask them to move it somewhere
permanent — their Documents folder or Desktop works well. Downloads tends to get cleaned out.

Then ask: "Now open the Terminal app — press Command + Space, type **Terminal**, and press Enter.
A window will open. Don't worry, you'll only paste one command."

Ask them to **drag the `setup.sh` file from their qbo-bookkeeper folder into the Terminal window**
(the path fills in automatically), type a space, and press Enter. The command will look like:

```
bash /Users/yourname/Documents/qbo-bookkeeper/setup.sh
```

Tell them what's going to happen: "The script will install a few Python packages (about 30
seconds), then ask for your Client ID and Client Secret from the Intuit Developer portal.
Type them in when prompted — the secret won't show as you type, which is normal."

Once they've run it, ask them to **paste the last few lines of Terminal output here** so you
can verify it worked. A successful run ends with something like:
```
✅ Credentials stored at ~/.bookkeeper-qbo/config.json
```
and then an MCP config snippet that starts with `"bookkeeper-qbo": {`.

**If they don't have their Client ID / Secret yet:**
"Before we can finish, you'll need to complete Part 1 of the setup guide — creating your
Intuit Developer app and getting your Production credentials. Once you have your Client ID
and Client Secret, come back and we'll pick up right here."

**If they see "command not found: pip3" or "pip3: command not found":**
Python 3 isn't installed. Direct them to python.org to download and install Python 3 first,
then re-run setup.sh.

**If they see "error: externally-managed-environment":**
This is a Mac Homebrew Python quirk. Ask them to paste this command in Terminal instead:
```
pip3 install mcp httpx --break-system-packages --quiet
```
Then have them re-run setup.sh.

Once you can see the MCP config snippet in their Terminal output, move to Step 2.

---

## Step 2 — Add the Bookkeeper to Cowork and restart

**When:** Credentials are stored (CREDS_EXIST or just completed Step 1), MCP not yet configured.

### Step 2a — Copy the config snippet

The MCP snippet from setup.sh looks like this (their path will be different):
```json
"bookkeeper-qbo": {
  "command": "python3",
  "args": ["/Users/yourname/Documents/qbo-bookkeeper/mcp-server/server.py"]
}
```

Ask them to copy that entire block from their Terminal output. They'll need it in the next step.

### Step 2b — Add it to Cowork

Tell them: "In Cowork, go to **Settings → MCP Servers** (sometimes labeled Plugins or 
Extensions). You should see a JSON editor. Add the snippet you just copied inside the 
`mcpServers` block, so it looks like this:

```json
{
  \"mcpServers\": {
    \"bookkeeper-qbo\": {
      \"command\": \"python3\",
      \"args\": [\"/Users/yourname/Documents/qbo-bookkeeper/mcp-server/server.py\"]
    }
  }
}
```

If there are already other entries in mcpServers, just add a comma after the last one and
paste the bookkeeper-qbo block. Then save."

### Step 2c — Restart Cowork

"Now **quit Cowork completely** — right-click the icon in your Dock and choose Quit, or press
Command + Q. Then reopen it. This is the step most people miss — just closing the window isn't
enough, the Bookkeeper needs a full restart to load."

"Once you're back in Cowork, come back to this chat and say **'I restarted'** — we'll finish
connecting to QuickBooks."

---

## Step 3 — Connect to QuickBooks (OAuth)

**When:** Client says "I restarted" or has credentials + MCP configured + no tokens yet.

While they're restarting, note the time. When they come back, acknowledge it:
"Welcome back! Now the fun part — let's connect the Bookkeeper to your QuickBooks account."

**Before calling qbo_authenticate**, give them this heads-up:
"Once your browser opens, you'll have **10 minutes** to sign in and paste the URL back here.
Just something to know so you don't get distracted and have to start over."

Call `qbo_authenticate`. It will open the client's browser to Intuit's sign-in page.

Then guide them through the browser steps:
"Your browser just opened to the Intuit sign-in page. Here's what to do:
  1. Sign in with your QuickBooks account (email + password)
  2. Choose the QuickBooks company you want the Bookkeeper to manage
  3. Click the blue **Connect** button
  4. You'll land on a page that may look strange — it's called the Intuit OAuth Playground.
     That's expected — don't close it!
  5. **Look at the address bar at the very top of your browser.** The URL will start with
     `https://developer.intuit.com/app/developer/playground?code=...` and be quite long.
     Click in that bar, select all (Command + A), copy it, and paste the entire thing here."

**If the browser didn't open automatically:**
"If your browser didn't open, let me know — I'll give you a link to paste into it manually."
(Call qbo_authenticate again and share the returned URL for them to open themselves.)

**When they paste the URL:**
Parse it to extract `code` and `realmId`. A correct URL looks like:
```
https://developer.intuit.com/app/developer/playground?code=AB11...&realmId=9130...&state=bookkeeper-qbo-auth
```

**If what they pasted doesn't look like a URL** (e.g., it's a page title, partial text, or
just the word "playground"): gently clarify — "That looks like it might be the page title
rather than the URL! What I need is the full web address from the address bar — it starts
with `https://` and is quite long. Try clicking in the address bar, selecting everything
with Command + A, and copying that."

Once you have the URL, call:
```
qbo_complete_auth(auth_code="[extracted code]", realm_id="[extracted realmId]")
```

**If you get `invalid_grant`:** The code expired. Reassure them:
"No problem at all — the code is only good for 10 minutes, and it looks like it timed out.
Let's just run it again." Call `qbo_authenticate` again and walk them through it once more.
Remind them to paste the URL as soon as they land on the Playground page.

**If you get `invalid_client`:** The credentials in setup.sh were entered incorrectly (usually
a copy-paste error). Ask them to re-run setup.sh and carefully re-enter their Client ID and
Secret, then try the OAuth flow again.

---

## Step 4 — Verify the connection

**When:** TOKENS_EXIST, or just completed Step 3.

Call `qbo_get_company_info`.

**If it returns company info:**

Check the company name matches what the client expects. Then celebrate:

"🎉 the Bookkeeper is connected to **[Company Name]**! Your AI bookkeeper is live.

Here are some things you can ask the Bookkeeper right now:
  • 'What were my total expenses last month?'
  • 'Show me my profit and loss for this year'
  • 'Run a balance sheet as of today'
  • 'What vendors do I have set up?'
  • 'What's in my undeposited funds account?'

Just describe what you need in plain English — the Bookkeeper will pull it straight from QuickBooks.
What would you like to look at first?"

**If the company name is wrong** (connected to the wrong QuickBooks company):
"It looks like the Bookkeeper connected to [Wrong Company] — let's fix that. Run `qbo_authenticate`
again and this time make sure to select [Correct Company] from the company picker before
clicking Connect."

**If it fails with an auth error:**
Tokens may be stale or corrupted. Go back to Step 3 and re-run the OAuth flow.

---

## Tone and approach

- Be warm and patient — many clients find Terminal intimidating, and that's completely normal
- Explain *why* each step matters in one sentence — it builds confidence and reduces anxiety
- When a step works, acknowledge it explicitly: "Perfect — that worked!"
- When something fails, stay calm and curious: "Let's figure this out"
- Never make the client feel like they did something wrong — frame errors as expected bumps
- Keep it conversational. You're a knowledgeable friend walking them through this, not a manual
- Avoid jargon (OAuth, MCP, TTY) unless the client is clearly technical — use plain language
- The goal is for the client to finish feeling capable, not just relieved
