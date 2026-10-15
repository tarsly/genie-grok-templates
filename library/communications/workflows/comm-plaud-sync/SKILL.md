---
name: comm-plaud-sync
description: >-
  Use when the user says '/plaud-sync', 'sync my PLAUD', 'sync my voice recordings', or wants to pull recent PLAUD recordings into Cloud Brain. Calls the Plaud MCP directly, classifies each recording by domain (professional, personal, project-specific, research), extracts action items and insights, and routes structured notes to the right Cloud Brain folders. Designed to run on a daily or weekly schedule as part of a communications agent workflow.
---

# COMM-Plaud-Sync
## PLAUD Voice Recording Sync

---

## Overview

This skill syncs AI-generated summaries from PLAUD voice recordings into your Cloud Brain — automatically classifying each recording by domain, extracting action items and insights, and routing notes to the right place. Designed to run on a daily or weekly schedule as part of a communications agent workflow.

**How it works:** Calls Plaud's official MCP server directly to pull recordings, metadata, and AI summaries. No browser, no scraping, no manual logins after the first authorization. The skill analyzes each new recording, classifies it by domain, and saves structured notes to your Cloud Brain.

**Connectors required:**
- **Plaud MCP** — official remote MCP server at `https://mcp.plaud.ai/mcp` (one-click install in Cowork)
- **Cloud Brain MCP** — for saving notes (included with all Genie plugins)

**No regulated disclaimer required for this skill.**

> Tool-name caveat: this document uses `mcp__plaud__*` for the Plaud MCP tool prefix. The actual prefix in your client is `mcp__<connector-slug>__<tool>` where `<connector-slug>` is whatever name you used when adding the connector. If you named it differently (e.g. "Plaud Web MCP"), run `claude mcp list` (Claude Code) or check Settings → Connectors (Cowork) to see the exact name, and use that prefix.

---

## First-Time Setup (New Clients)

Run this section on first use. It walks through everything needed before syncing can happen automatically.

### Step 1 — Check for Saved Preferences

Search Cloud Brain: `mcp__cloud-brain__search_notes` with query "plaud preferences"

If preferences are found: confirm in ONE line — "Syncing PLAUD for [Name], [N]-day window, correct?"
- If yes: skip to Pre-Flight
- If no: continue setup

If no preferences found: run the full onboarding below.

---

### Step 2 — PLAUD Device & App Setup

Ask the client:

> "Before we connect your PLAUD recordings, let's make sure everything is set up. Do you already have a PLAUD device and an active account at plaud.ai?"

**If yes:** Proceed to Step 3.

**If no, or if they're unsure:** Provide this onboarding overview:

```
PLAUD SETUP OVERVIEW

PLAUD is an AI voice recorder that automatically transcribes and summarizes
your meetings, ideas, and voice notes. Here's what you need:

HARDWARE:
  PLAUD NOTE — pocket-sized card that attaches to your phone (most common)
  PLAUD NotePin — wearable clip-on recorder

MOBILE APP:
  Download "PLAUD" from the App Store or Google Play
  Create an account at plaud.ai (same login works on web and mobile)

RECORDING WORKFLOW:
  1. Press the button on your PLAUD device to start recording
  2. When done, the recording syncs to the PLAUD app automatically
  3. PLAUD generates an AI summary within a few minutes
  4. This skill then pulls those summaries into your Cloud Brain

SUBSCRIPTION NOTE:
  PLAUD's AI summary feature requires a PLAUD subscription.
  Free accounts get limited AI processing per month.
  Check plaud.ai/pricing for current plan details.

Once you have your device and account set up, come back and we'll
connect the sync.
```

Stop here if the client needs to get their PLAUD set up first. Tell them: "Once your PLAUD account is active and you have some recordings, come back and say 'plaud sync' to continue setup."

---

### Step 3 — Plaud MCP Connector Check

Probe whether the Plaud MCP is available by attempting `mcp__plaud__list_files` with `limit: 1`.

**If the tool is not registered (raises "tool not found" or similar):**

```
PLAUD MCP REQUIRED

This skill talks to PLAUD directly through their official MCP server —
no browser scraping, no manual logins after the first authorization.

CLAUDE DESKTOP / COWORK (recommended):
  1. Open claude.ai → Settings → Connectors
  2. Click "Add Custom Connector"
  3. Name: Plaud
     URL:  https://mcp.plaud.ai/mcp
  4. Click Connect — you'll be redirected to PLAUD for OAuth login
  5. Authorize and return to this chat

CLAUDE CODE (terminal):
  claude mcp add --transport http plaud https://mcp.plaud.ai/mcp
  Then run the skill once — it will prompt the OAuth flow on first use.

Once connected, all your recordings, summaries, and transcripts are
available to this skill instantly. Come back and say "plaud sync".
```

Stop here until the user confirms the connector is installed.

**If the tool is registered but the call returns an auth error** (401, "not authenticated", "needs reauthorization"):

```
PLAUD MCP NOT AUTHORIZED

The connector is installed but hasn't been signed in.
Open Settings → Connectors → Plaud → click Reconnect.
You'll be redirected to PLAUD's OAuth page — sign in and authorize.
Then come back and say "plaud sync".
```

Stop here until the user confirms reauthorization.

**If the call returns a normal response** (even an empty list): the connector is ready. Proceed to Step 4.

---

### Step 4 — Save Preferences

Collect the following in ONE message:

- **Your name** — for attribution in notes
- **Default sync window** — how many days back to pull on each run (default: 7)
- **Project keywords** — any active project names or keywords you want recordings matched against (e.g., "Acme deal", "Q3 launch", "coaching program")
- **Output format** — Full notes with all sections, or compact notes (summary + action items only)?
- **Include transcripts?** — Default is summaries only. Set this to "yes" to also pull and store the speaker-diarized transcript per recording.

Save to Cloud Brain using `mcp__cloud-brain__write_note` with folder `preferences`, title `comm-preferences` (appended to existing comm preferences if they exist, or created fresh).

Show confirmation: "Setup complete — preferences saved. Running your first sync now..."

Proceed to Core Flow.

---

## Pre-Flight (Returning Users)

### 1. Load Preferences

Search Cloud Brain for `plaud preferences`. Apply saved settings:
- Sync window (days)
- Project keywords
- Output format preference
- Transcript inclusion (default: off)

### 2. Get Current Timestamp

Run `date '+%Y-%m-%d %H:%M'` via Bash. Store for use in note metadata. Never fabricate timestamps.

### 3. Connector Health Check

Call `mcp__plaud__list_files` with `limit: 1`.

- **Returns a result (even empty):** proceed.
- **Returns auth error:** show the "PLAUD MCP NOT AUTHORIZED" message from Step 3 above. Stop.
- **Tool not registered:** show the "PLAUD MCP REQUIRED" install instructions from Step 3. Stop.

### 4. Parse Arguments

- `--days N` — Override saved sync window for this run only
- `--project filter` — Only process recordings matching this keyword
- `--include-transcript` — Also fetch full transcripts (off by default)
- `--dry-run` — Show routing plan without writing any notes

---

## Core Flow

### Step 1 — List Recent Recordings

Call `mcp__plaud__list_files` with a date filter for the sync window. Use the saved/parsed `--days N` value to compute the start date (today minus N days, in UTC).

If `list_files` supports a `since` / `from_date` parameter, use it. Otherwise pull a generous page and filter client-side by `started_at` from the returned metadata.

Filter the returned list to recordings within the sync window. If empty:

```
No PLAUD recordings found in the past [N] days. Nothing to sync.
```

Exit cleanly.

---

### Step 2 — Deduplication

For each recording's `file_id`, call `mcp__cloud-brain__search_notes` with query `plaud_file_id:[id]`.

Skip any recording whose `plaud_file_id` already exists in a Cloud Brain note.

If every recording is a duplicate:

```
All PLAUD recordings from the past [N] days have already been synced. Nothing new to import.
```

Exit cleanly.

---

### Step 3 — Process Each Recording

#### 3a — Fetch Metadata + Summary

For each new recording, in series:

1. `mcp__plaud__get_file(file_id)` — returns title, duration, `started_at`, and other structural metadata.
2. `mcp__plaud__get_note(file_id)` — returns Plaud's AI summary, key topics, and action items as structured fields. This is the primary content source.
3. If `--include-transcript` is set: `mcp__plaud__get_transcript(file_id)` — returns the speaker-diarized transcript with timestamps. Otherwise skip.

If `get_note` returns an empty/null summary (Plaud hasn't finished processing yet), create a minimal note with metadata only and flag: `⚠️ Summary not yet available — re-sync later.` Continue to the next recording.

No rate limiting, no retries, no session checks — the MCP server handles all of that.

---

#### 3b — Domain Classification

Use `mcp__cloud-brain__search_notes` with query "project" to find active projects. Collect names, slugs, and keywords.

**Classification rules:**

| Domain | Signals |
|--------|---------|
| `project-specific` | Matches an active project (requires 2+ signals: name + context, or multiple keyword hits) |
| `professional` | Work-related, not tied to a specific project |
| `personal` | Personal reflections, life planning, relationships, health |
| `research` | Market data, competitor info, statistics, learning |
| `mixed` | Spans multiple domains, or confidence < 70% |

A recording can match at most one project. If ambiguous between projects, default to `professional`. When confidence < 70%, classify as `mixed`.

---

#### 3c — Content Analysis

**Prefer Plaud's structured fields over re-extracting from prose.** The `get_note` response includes Plaud's own `action_items`, `key_topics`, and `summary`. Use them as-is. Fall back to LLM extraction from the summary text only when those arrays are empty or missing.

Map Plaud's structured output to the note's analytical sections:

- **Summary** ← Plaud's `summary` field, verbatim
- **Key Themes** ← Plaud's `key_topics` (or LLM-derived top 3–5 if absent)
- **Action Items** ← Plaud's `action_items` (attributed to the user by default unless ownership is clear from the text)
- **Insights / Decisions / People Mentioned / Questions Raised** ← LLM-derived from the summary text

For action items, normalize owner attribution: use names from the recording when available; use "Unassigned" when ownership is unclear.

---

#### 3d — Generate Note

```markdown
---
type: "plaud-note"
domain: "[personal|professional|project-specific|research|mixed]"
project: "[project-slug or empty]"
date: "YYYY-MM-DD"
created: "YYYY-MM-DD HH:MM"
plaud_file_id: "[file-id]"
recording_started: "YYYY-MM-DD HH:MM"
recording_duration: "[duration in minutes]"
themes: ["theme1", "theme2", "theme3"]
tags: ["plaud", "voice-note", "[domain-tag]"]
status: "captured"
transcript_included: false
---

# PLAUD Note: [Descriptive Title from Content]

## Summary
[Plaud's AI-generated summary, verbatim]

## Key Themes
1. **[Theme]:** [description]
2. **[Theme]:** [description]
3. **[Theme]:** [description]

## Insights
- [Notable insight]
- [Notable insight]

## Action Items
- [ ] [Action item] — [Owner]
- [ ] [Action item] — [Owner]

## Decisions
- [Decision with context]

## People Mentioned
- **[Name]:** [context of mention]

## Questions Raised
- [Open question]

## Domain Classification
- **Primary Domain:** [domain] ([confidence]%)
- **Reasoning:** [brief rationale]

---
*Synced from PLAUD via Communications Suite — Plaud Sync v2.0 (MCP)*
```

If `--include-transcript` is set, append a final section:

```markdown

## Transcript

[Speaker-diarized transcript from `mcp__plaud__get_transcript`, formatted as
`[HH:MM:SS] Speaker: text...` lines]
```

And set `transcript_included: true` in the frontmatter.

YAML frontmatter rules: all strings quoted with double quotes; arrays use quoted strings; numbers unquoted unless they are string identifiers.

---

### Step 4 — Save to Cloud Brain

All notes saved using `mcp__cloud-brain__write_note`. Per Cloud Brain conventions, **never** include a `brain/` prefix in the `folder` parameter — pass folder names directly.

#### Primary Note

- **folder:** `transcripts`
- **title:** `PLAUD Note YYYY-MM-DD [Descriptive Slug]` (e.g., `PLAUD Note 2026-04-07 Team Strategy Discussion`)
- **tags:** `["plaud", "voice-note", "<domain>"]`
- **content:** the full structured note from Step 3d

Include `[[Entity Name]]` wiki-links for all people mentioned and projects referenced.

#### Cross-Reference Updates

| Condition | Also update |
|-----------|-------------|
| All recordings | Append entry to `daily/Daily Log YYYY-MM-DD` with a `[[PLAUD Note YYYY-MM-DD Slug]]` link |
| `project-specific` | Find the project note via `mcp__cloud-brain__search_notes`, append a cross-reference link |
| `research` | Save or update a topic note under folder `research` |

#### People Mentioned

For each new person with substantive context (role, company, relationship):
1. Check `mcp__cloud-brain__search_notes` for an existing people entry
2. If new: use `mcp__cloud-brain__write_note` with folder `people`, title `Firstname Lastname`, content including context and a link back to the PLAUD note

---

### Step 5 — Sync Summary

After all recordings are processed:

```
PLAUD SYNC COMPLETE — [Date]

| # | Recording | Domain | Duration |
|---|-----------|--------|----------|
| 1 | [title]   | Professional | 12 min |
| 2 | [title]   | Project: [name] | 8 min |
| 3 | [title]   | Personal | 5 min |

Imported: [N] new recordings
Skipped:  [N] (already synced)
Total audio processed: [N] minutes
Brain notes updated: [list any daily logs, project notes, research notes touched]
```

If any recordings failed, list them with the file ID and error details.

**After sync, you may want to run:**
- **Bizops Daily Brief** — see how new recordings connect to today's priorities
- **Comm-Meeting-Actions** — extract deeper action items from any formal meeting recording
- **Comm-Meeting-Transcript** — full debrief processing for high-stakes calls

---

## Scheduling — Run Automatically

This skill is designed to run on a schedule without manual triggering. Recommended setups:

**Daily (most current):** Run each morning at 6:00 AM with `--days 1` — catches only yesterday's recordings and delivers the digest before you start your day.

**Weekly (less noise):** Run Monday mornings with `--days 7` — reviews the full prior week in one batch.

To set up automated syncing, run the **AI Agents** plugin and assign Plaud Sync to your Communications Agent with your preferred schedule.

---

## Error Handling

| Scenario | Response |
|----------|----------|
| Plaud MCP not installed | Show the "PLAUD MCP REQUIRED" install instructions. Stop. |
| Plaud MCP installed but unauthorized | Show the "PLAUD MCP NOT AUTHORIZED" reauthorization message. Stop. |
| `list_files` empty for the sync window | "No PLAUD recordings in the past [N] days. Nothing to sync." Exit cleanly. |
| `get_note` returns empty summary | Create minimal note with metadata only. Flag: "⚠️ Summary not yet available — re-sync later." |
| `get_transcript` fails when `--include-transcript` is set | Save the note with summary fields populated and `transcript_included: false`; mention the transcript failure in the sync summary. |
| MCP transport error (network, 5xx, timeout) | Surface the error verbatim. Suggest re-running. Never fabricate or guess at content. |
| `--dry-run` | Show the full routing plan (which notes would be created, which brain notes updated) without writing anything. |

---

## Deduplication Rules

- Always deduplicate by `plaud_file_id`, never by title or date alone
- A recording is a duplicate if and only if its exact `plaud_file_id` exists in any Cloud Brain note's frontmatter
- Re-running the skill is always safe — it will skip everything already synced
- Frontmatter schema (`plaud_file_id`, `domain`, etc.) is unchanged from earlier versions of this skill, so dedup against historically-synced notes works without migration
