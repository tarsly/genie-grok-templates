# Communications Suite
### Version 2.2.0 — the provider

---

## What This Plugin Does

The Communications Suite is an AI-powered toolkit that handles the most time-consuming parts of professional communication: processing your inbox, drafting emails in your voice, walking into every meeting prepared, extracting decisions from transcripts, and building negotiation strategies on demand.

The flagship use case is the **Executive Inbox Agent** — a scheduled agent that triages your inbox every morning before you start your day. You wake up to a structured digest: what's urgent, what can wait, what to ignore — with draft responses ready for critical items.

---

## Skills in This Plugin

### Comm-Inbox-Triage
**Trigger phrases:** "triage my inbox," "process my email," "inbox zero," "what needs a response," "email triage," "clear my inbox"

Processes a batch of emails and categorizes every item into four tiers:
- 🚨 **Urgent / Action Required** — needs a response today
- ⚡ **Important / Respond This Week** — needs attention but not today
- 📋 **Low / Read or File** — informational, no reply needed
- 🗑️ **Ignore / Unsubscribe Candidate** — noise to eliminate

Drafts responses for urgent items. Surfaces action items. Flags repeat noise senders for unsubscribing. Designed to run on a daily schedule — this is the core skill of the Executive Inbox Agent.

---

### Comm-Email-Drafter
**Trigger phrases:** "draft an email," "write an email to," "cold outreach email," "follow-up email," "proposal email," "write this email for me"

Drafts professional emails for any situation in your tone and voice. Handles cold outreach, follow-ups, proposals, introductions, thank-yous, negotiation emails, investor outreach, and client check-ins. Delivers the draft ready to send, with structure notes and alternate versions.

---

### Comm-Meeting-Prep
**Trigger phrases:** "prep for my meeting," "meeting prep," "meeting with [name]," "talking points for," "prepare for my call"

Generates a complete pre-meeting intelligence brief: attendee research, structured agenda, key talking points, anticipated objections with scripts, questions to ask, and a follow-up email template — ready before you walk in the door.

---

### Comm-Meeting-Transcript
**Trigger phrases:** "process this transcript," "meeting transcript," "what did we decide," "summarize this recording," "debrief this call"

Paste a transcript or recording summary and get a structured debrief: decisions made, action items with owners, follow-ups needed, open questions, and key insights. Cross-references against your prep brief if one was saved.

---

### Comm-Meeting-Actions
**Trigger phrases:** "extract action items," "action items from this meeting," "who owns what," "parse these meeting notes"

The fast-path version of Meeting-Transcript, purpose-built for converting notes into a task list. Paste meeting notes and get structured action items with owners, due dates, and a shareable summary block ready to paste into Slack or email.

---

### Comm-Negotiation-Prep
**Trigger phrases:** "negotiation prep," "negotiation strategy," "how should I negotiate," "prep for this deal," "contract negotiation," "salary negotiation"

Builds a complete negotiation strategy: BATNA analysis, leverage map, counterparty research, objection scripts, concession strategy, and a structured conversation flow guide. Works for any negotiation — salary, vendor contracts, deal terms, partnerships, pricing.

---

### Comm-iMessage-Fast *(bundled MCP server — recommended)*
**Trigger phrases:** "send a text," "iMessage," "text someone," "send an iMessage," "what's new in iMessage," "what did [name] text me"

Send, read, and search iMessages via a bundled `imessage-fast` MCP server that reads `~/Library/Messages/chat.db` directly and sends through AppleScript to `Messages.app`. No BlueBubbles, no localhost server, no passwords. Always shows the message for your confirmation before sending.

**Requirements (macOS only):**
- Node.js LTS ≥ 20 (`brew install node` if missing)
- Full Disk Access granted to the process that launches Claude — see setup skill below
- Messages.app open and signed in

---

### Comm-iMessage-Fast-Setup *(one-time post-install)*
**Trigger phrases:** "set up iMessage," "install iMessage plugin," "iMessage isn't working," "why can't Claude text"

The first-time setup companion to `comm-imessage-fast`. Auto-invoked when any iMessage tool fails with a dependencies / MCP-server / Full-Disk-Access error, or explicitly when the user says something like "set up iMessage."

**What it does:**
- Finds the plugin install (works in both Claude Code and Claude Desktop paths)
- Verifies Node.js ≥ 20
- Runs the bundled `setup.sh` to install runtime dependencies
- Walks the user through granting Full Disk Access (to `Claude.app` in Desktop, or to the terminal in Claude Code)
- Verifies the MCP tools come online after a Claude restart

Idempotent — safe to re-run any time. Users can also copy-paste the one-liner from the skill for a manual setup path.

---

### Comm-iMessage *(legacy — BlueBubbles path)*
**Trigger phrases:** *(prefer `comm-imessage-fast` above)*

Legacy skill kept for users still running BlueBubbles. New activations should use `comm-imessage-fast`, which needs no BlueBubbles server, no port 1234, and no password.

---

## The Executive Inbox Agent

The highest-value use of this plugin is setting up an **Executive Inbox Agent** using the `ai-agents` system:

1. Run **system-03 (Agent Designer)** and choose the "Executive Inbox Agent" configuration
2. Run **system-04 (Agent Activator)** to set your preferences and schedule the daily 6:30 AM triage
3. Every morning you'll receive a triage digest before you open your laptop — urgent items with draft responses, everything else categorized and cleared

Optionally add `bizops-daily-brief` to the same agent to combine your inbox triage with your daily priorities into one unified morning briefing.

---

## Preferences Setup

On first run, each skill collects your preferences in one message and saves them to your Cloud Brain. All comm skills share a single preference file, so you only set up once.

**What gets saved:**
- Your name, title, and business
- Email accounts and triage settings (VIP list, ignore list, response time)
- Email tone and signature
- Meeting and follow-up style preferences
- Negotiation style and domains

**To update your preferences at any time:** Tell any comm skill "update my communications preferences" and it will walk you through the changes.

---

## Memory Paths

| Data | Cloud Brain Path |
|---|---|
| Preferences | `brain/preferences/comm-preferences.md` |
| Daily triage logs | `brain/communications/triage/triage-[date].md` |
| Email drafts | `brain/communications/drafts/draft-[slug]-[date].md` |
| Meeting prep briefs | `brain/communications/meetings/prep-[slug]-[date].md` |
| Meeting debriefs | `brain/communications/meetings/debrief-[slug]-[date].md` |
| Negotiation strategies | `brain/communications/meetings/negotiation-prep-[slug]-[date].md` |

---

## Pairs Well With

- **business-operations** — Route meeting action items to the Follow-Up Tracker; combine inbox triage with the Daily Brief for a unified morning briefing
- **ai-agents** — Set up the Executive Inbox Agent with a daily triage schedule
- **market-intelligence** — Meeting Prep uses deep research for high-stakes attendee profiles

---

## Version History

### v2.2.0 — 2026-06-30
Added `comm-imessage-fast-setup` — the one-time post-install setup skill for `imessage-fast`. Locates the plugin install (Claude Code + Claude Desktop paths), runs `setup.sh`, guides FDA grant, and verifies tools come online. Auto-invoked by `comm-imessage-fast` on any dependencies / FDA error. Ten skills total.

### v2.1.0 — 2026-06-30
Added `comm-imessage-fast` — native iMessage via a bundled `imessage-fast` MCP server. Replaces the BlueBubbles dependency for send/read/search. `comm-imessage` (BlueBubbles) is retained for existing users but marked legacy in AGENTS.md and its SKILL.md. Nine skills total.

### v2.0.2 — earlier
`comm-plaud-sync` added. Eight skills.

### v1.0.0 — 2026-06-12
Initial release. Seven skills: inbox triage, email drafting, meeting prep, meeting transcript processing, meeting action extraction, negotiation prep, and iMessage. Shared preferences layer. Executive Inbox Agent configuration in AGENTS.md.

---

*the provider — communications v2.2.0*


## Grok Bot import
See the root README. Replace `{{BOT_NAME}}` in `system-prompt.md` and `workflows/getting-started/SKILL.md`.
