# Zoom Suite — Agent Capabilities Manifest

*For use by system-03 (Agent Designer) and system-04 (Agent Activator)*
*Last updated: 2026-06-12*

---

## About This Plugin

`zoom-meetings` brings live Zoom data into the Genie workflow. Skills connect directly to the user's Zoom account via the Zoom MCP connector — pulling meeting summaries, transcripts, chat messages, and Zoom Docs without any manual copying or pasting. The plugin covers the full meeting lifecycle: prep before, debrief after, search across history, and weekly digest to close loops.

**Target users:** Any professional who runs meetings on Zoom — consultants, business owners, sales teams, real estate agents, coaches, operators. High value for anyone managing multiple client relationships where meeting history and follow-through matter.

**Requirement:** Zoom MCP connector must be installed and connected for all skills to function.

**Plugin version:** 1.0.0 (5 skills)

---

## Available Skills Catalog

---

### 0. comm-zoom-onboarding ← **Run First**

**What it does:** One-time setup that verifies the Zoom MCP connector is live, checks that Zoom AI Companion and Cloud Recording are enabled on the client's Zoom account, collects preferences for all four skills in a single interview, and runs a live test debrief on the most recent meeting. Catches misconfiguration before it causes silent failures on the other skills.

**Preferences collected at activation:** All preferences for the entire plugin — name, timezone, debrief settings, digest depth, and Zoom Doc preferences. Clients will not be asked again.

**Suggested schedule:** On-demand, one-time. Run at plugin activation; re-run anytime to verify connection status or update preferences.

**Natural pairings:** Should be the first skill run in any Zoom Suite activation sequence. system-04 should trigger this before activating any other skill in the plugin.

---

### 1. comm-zoom-debrief

**What it does:** After any Zoom meeting, the user says "debrief my last call" — the skill finds the meeting in Zoom, pulls the AI summary, full transcript, and Zoom's auto-generated next steps, then extracts decisions made, action items with owners and deadlines, follow-ups, and key insights. Saves to Cloud Brain and optionally creates a Zoom Doc. No pasting required.

**Preferences collected at activation:**
- User's name (to identify their action items)
- Default deadline for undated action items (e.g., "3 business days" or none)
- Whether to auto-create a Zoom Doc after every debrief

**Suggested schedule:** On-demand — triggered after any meeting ends.

**Natural pairings:** comm-zoom-prep (prep before the meeting, debrief after), comm-zoom-search (find a past debrief's outputs later), bizops-follow-up (track action items from debriefs).

---

### 2. comm-zoom-prep

**What it does:** Pre-meeting intelligence brief that pulls full Zoom history with the attendees, extracts prior decisions and open items from past calls, combines it with web research, and builds a complete brief: what was discussed last time, what's still unresolved, agenda, talking points, anticipated objections, and a ready-to-send follow-up template.

**Preferences collected at activation:**
- User's name, role, company
- Primary goal in most meetings (close deals, build partnerships, onboard clients, etc.)
- Preferred meeting length/format

**Suggested schedule:** On-demand — triggered before any client, partner, or sales meeting.

**Natural pairings:** comm-zoom-debrief (always run debrief after; debrief feeds the next prep).

---

### 3. comm-zoom-search

**What it does:** Natural language search across all Zoom meetings, recordings, transcripts, Team Chat, and Zoom Docs. "What did we decide about pricing in Q1?" returns the specific meeting, the decision, and the quote — with speaker and timestamp.

**Preferences collected at activation:** None — this is a pure retrieval skill with no stable identity preferences.

**Suggested schedule:** On-demand only. Not a scheduling candidate.

**Natural pairings:** comm-zoom-debrief (find a specific past meeting to debrief in full), comm-zoom-prep (check what was decided before prepping for a follow-up call).

---

### 4. comm-zoom-digest

**What it does:** Weekly executive digest of all recorded Zoom meetings. Summarizes each meeting, surfaces action items the user owns, flags items that were open from prior weeks and haven't been resolved, identifies recurring themes across meetings, and delivers a clean accountability report. Designed to be scheduled every Monday morning.

**Preferences collected at activation:**
- User's name
- Preferred digest time window (default: past 7 days)
- Digest depth: Brief or Detailed
- Save as Zoom Doc: Yes / No
- Any meeting categories to exclude from the digest

**Suggested schedule:** Every Monday at 7:00 AM (user's timezone). This is the core scheduled skill in the plugin.

**Natural pairings:** comm-zoom-debrief (dive deeper on any meeting flagged in the digest), bizops-daily-brief (open items from digest surface in daily brief).

---

## Preferences Registry

| Skill | Preference | Brain Path |
|---|---|---|
| comm-zoom-debrief | User name, default deadline, Zoom Doc auto-create | `brain/preferences/zoom-debrief-preferences.md` |
| comm-zoom-prep | User name, role, company, meeting goal, format | `brain/preferences/meeting-prep-preferences.md` |
| comm-zoom-digest | User name, time window, depth, Zoom Doc, exclusions | `brain/preferences/zoom-digest-preferences.md` |
| comm-zoom-search | No persistent preferences | — |

---

## Suggested Agent Configurations

---

### Option A: Meeting Intelligence Agent (Recommended)

One "Meeting Intelligence" agent handles all four skills.

**Best for:** Any professional managing client relationships, deals, or team projects over Zoom. The agent handles the full meeting lifecycle — prep → debrief → digest — and answers ad-hoc history questions.

**Activation interview:** Run all three preference collections (debrief, prep, digest) in a single setup session (~8 minutes total). All four skills will then run cleanly from the first use.

---

### Option B: Digest-Only Agent

One "Weekly Recap Agent" running comm-zoom-digest on a Monday schedule.

**Best for:** Users who want the weekly accountability layer without changing their current meeting prep or debrief workflow. Low setup cost, high ongoing value.

---

## Recommended Schedules

| Skill | Recommended Schedule | Notes |
|---|---|---|
| comm-zoom-debrief | On-demand | After any meeting. Also works retroactively for missed debriefs. |
| comm-zoom-prep | On-demand | Before any client or external meeting. |
| comm-zoom-search | On-demand | Never scheduled — always reactive. |
| comm-zoom-digest | Weekly — Monday 7:00 AM | Core scheduled skill. Run manually first to set preferences. |

---

## How system-03 Should Use This Document

Recommend this plugin to any client who:
- Manages external relationships (clients, partners, investors) over Zoom
- Struggles with post-meeting follow-through
- Wants accountability on action items across multiple recurring calls
- Has said anything like "I forget what we discussed last time" or "I lose track of follow-ups"

The plugin works for any industry. The digest skill alone justifies installation for any Zoom-heavy professional.

## How system-04 Should Use This Document

1. **Always run comm-zoom-onboarding first.** It verifies the Zoom MCP connector, checks AI Companion and Cloud Recording status, and collects all preferences in one session. Do not activate other skills until onboarding passes.
2. Preferences for all four skills are collected inside comm-zoom-onboarding — no separate preference interviews needed during activation.
3. Schedule comm-zoom-digest for Monday mornings immediately after onboarding completes.
4. The other three skills are on-demand — no scheduling needed.
