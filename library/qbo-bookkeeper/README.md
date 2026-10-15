# the Bookkeeper — AI Bookkeeper for QuickBooks Online

**Version 1.1.0 | the provider**

the Bookkeeper connects your QuickBooks Online account to Claude so you can manage your books in plain English.

---

## What the Bookkeeper Can Do

- Run profit & loss reports, balance sheets, and cash flow summaries
- List and search transactions, invoices, customers, and vendors
- Create invoices, record payments, log expenses and deposits
- Answer bookkeeping questions using your real QBO data

---

## Getting Started (Brand New)

**Step 1:** Run the `bookkeeper-onboarding` skill — say **"set up the Bookkeeper from scratch"**

the Bookkeeper will walk you through:
1. Creating a free Intuit Developer account (browser only, ~2 min)
2. Building a QuickBooks app and configuring it (~5 min)
3. Submitting for production approval (Intuit reviews in 1–5 business days)
4. Running the one-time setup script to connect everything
5. Authorizing the Bookkeeper to access your QuickBooks company
6. Verifying the live connection

---

## Already Have Credentials?

Run `setup-bookkeeper` instead — say **"connect the Bookkeeper to QuickBooks"** or **"set up the Bookkeeper"**.

---

## One-Time Setup Scripts

These run in Terminal once to configure the Bookkeeper on your Mac:

| Script | Purpose |
|--------|---------|
| `setup.sh` | Installs Python dependencies and securely stores your Intuit credentials |
| `add-to-mcp-config.sh` | Registers the Bookkeeper with Cowork so it loads automatically |

After running both scripts, restart Cowork and run `qbo_authenticate` to complete the QBO connection.

---

## What's New in v1.1.0

- Added `bookkeeper-onboarding` skill — complete zero-to-live onboarding for brand new clients, including Intuit developer app creation and production approval guidance

## v1.0.0

- Initial release: MCP server, `setup-bookkeeper` skill, OAuth connection flow

---

## Privacy & Security

- Your Intuit credentials are stored locally at `~/.bookkeeper-qbo/config.json` (owner-read-only)
- OAuth tokens are stored at `~/.bookkeeper-qbo/tokens.json` and auto-refreshed
- No credentials are ever stored in Cowork or sent to Genie servers

---

*the Bookkeeper is powered by the Intuit QuickBooks Online API and the Genie Plugin Platform.*


## Grok Bot import
See the root README. Replace `{{BOT_NAME}}` in `system-prompt.md` and `workflows/getting-started/SKILL.md`.
