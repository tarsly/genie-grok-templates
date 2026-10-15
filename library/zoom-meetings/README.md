# Zoom Suite

**Live Zoom meeting intelligence for Genie clients**  
*Version 1.0.0 — Requires Zoom MCP connector*

---

## What This Plugin Does

Zoom Suite connects directly to your Zoom account and turns your meetings into a searchable, actionable knowledge base. Instead of copying transcripts, hunting through recordings, or forgetting what was decided last time — you just ask.

Five skills covering the full meeting lifecycle:

| Skill | When to Use |
|---|---|
| **comm-zoom-onboarding** | **Run first** — verifies setup, collects all preferences, runs a live test |
| **comm-zoom-debrief** | After any meeting — pulls transcript + AI summary, extracts decisions and action items |
| **comm-zoom-prep** | Before any meeting — shows your history with those people + builds a full prep brief |
| **comm-zoom-search** | Any time — "what did we decide about X?" searches meetings, chat, and docs |
| **comm-zoom-digest** | Every Monday — automated weekly recap of all your meetings + open items |

**Nothing to paste. All data comes live from Zoom.**

---

## Requirements

- **Zoom MCP connector** must be installed and connected. This plugin will not function without it.
- Zoom AI Companion enabled on your Zoom account (for AI summaries and transcripts). If AI Companion is not enabled, skills fall back to available metadata.
- Cloud Recording enabled in your Zoom settings (for comm-zoom-digest).

---

## Quick Start

**Step 1 — Run onboarding first:** Say "set up Zoom Suite" or "zoom onboarding". This verifies your Zoom connection, collects all your preferences in one session (~5 minutes), and runs a live test debrief so you can see the plugin working before you rely on it.

**Step 2 — Try the skills:**
- "Debrief my last Zoom call"
- "Prep me for my meeting with [name] on [topic]"
- "What did we decide about [topic]?"
- "Weekly meeting digest" (or schedule it for every Monday morning)

---

## Skill Details

### comm-zoom-onboarding

Run once after installing the plugin. Verifies three things: the Zoom MCP connector is live and authenticated, Zoom AI Companion is generating meeting summaries on your account, and Cloud Recording is enabled. If anything is off, it tells you exactly where to fix it in your Zoom account settings.

After verification, it collects preferences for all four skills in one interview — your name, timezone, action item deadline defaults, digest depth, and Zoom Doc settings. Then it runs a live debrief on your most recent meeting so you can see everything working before you depend on it.

**Re-run anytime** to check connection status or update your preferences.

---

### comm-zoom-debrief

Automatically finds a meeting in your Zoom account by name, person, topic, or date. Pulls the Zoom AI summary and full transcript. Extracts:
- Decisions made (with who made them)
- Action items (with owners, deadlines, and priority)
- Follow-ups still needed
- Key insights worth keeping

Saves everything to Cloud Brain and optionally creates a Zoom Doc for team visibility.

**Try it:** "Debrief my meeting with Sarah from yesterday" or "Process the pricing call from last week"

---

### comm-zoom-prep

Before any call, this skill goes back through your Zoom history with that person or company, pulls what was discussed and what's still unresolved, and combines it with web research on the person and their company. You walk in knowing:
- What you talked about last time
- What you said you'd do (and whether you did it)
- What they care about based on research
- A suggested agenda, talking points, and anticipated objections
- A follow-up email template ready to send after

**Try it:** "Prep me for my call with [Name] about [topic]"

---

### comm-zoom-search

Natural language search across everything in your Zoom account: meeting summaries, full transcripts, Team Chat messages, and Zoom Docs.

**Try it:**
- "What did we decide about pricing in Q1?"
- "Did [Name] ever bring up budget concerns?"
- "Find the meeting where we talked about the contract"
- "Search my Zoom chat for messages about [topic]"

Results come back with the exact meeting, date, attendees, and quote — so you can trace every decision back to its source.

---

### comm-zoom-digest

A weekly executive digest of all your recorded Zoom meetings. Every Monday morning:
- Summary of each meeting from the past week
- All action items assigned to you, consolidated in one place
- Open items carried over from prior weeks (the accountability layer)
- Cross-meeting themes worth noticing

Saves to Cloud Brain. Optionally creates a Zoom Doc.

**Recommended:** Schedule this to run automatically every Monday at 7:00 AM. First run, set your preferences; after that it runs silently and delivers the digest.

---

## How to Update Preferences

Each skill has its own preferences stored in Cloud Brain. To update any preference, run the skill and say "update my preferences" — the skill will walk you through the fields and save the changes.

---

## What Changed in This Version

**v1.0.0** — Initial release.
- comm-zoom-debrief: Live debrief with write-back to Zoom Docs
- comm-zoom-prep: History-aware prep brief with Zoom meeting lookup
- comm-zoom-search: Natural language search across meetings, chat, and docs
- comm-zoom-digest: Weekly digest with open-item tracking and scheduling support

---

## Why This Is Different from the Existing Meeting Skills

The existing `comm-meeting-transcript` and `comm-meeting-actions` skills require you to paste transcript text manually. Zoom Suite eliminates that entirely — all four skills pull data live from your Zoom account.

The existing `comm-meeting-prep` has no access to your Zoom history. Zoom Suite's prep skill knows what happened in every prior meeting with that person.

Install Zoom Suite when you're ready to stop copying transcripts and start working with your meeting data automatically.


## Grok Bot import
See the root README. Replace `{{BOT_NAME}}` in `system-prompt.md` and `workflows/getting-started/SKILL.md`.
