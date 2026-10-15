---
name: comm-zoom-onboarding
description: >-
  Zoom Suite setup — run this first before using any other Zoom Suite skill. Verifies that the Zoom MCP connector is live and authenticated, checks that Zoom AI Companion and Cloud Recording are enabled on your Zoom account, collects your preferences for all four skills in one session, and runs a live test debrief on your most recent meeting so you can see the plugin working before you rely on it. Triggers on: 'set up zoom suite', 'zoom setup', 'zoom onboarding', 'configure zoom', 'zoom suite setup', 'get zoom ready', 'install zoom suite', 'zoom prerequisites', 'test zoom connection', 'first time zoom suite'.
---

# COMM-ZOOM-ONBOARDING — Zoom Suite Setup & Verification

> **Run this first.** This skill sets up everything the other four Zoom Suite skills need.
> Takes approximately 5–10 minutes. Run it once — you won't be asked these questions again.

---

## Overview

This skill does three things in sequence:

1. **Verifies** that your Zoom account and MCP connection are correctly configured
2. **Collects** your preferences for all four Zoom Suite skills in a single interview
3. **Proves it works** by running a live debrief on your most recent meeting

If anything is misconfigured, this skill tells you exactly what to fix and where to find it in Zoom — before you try to use the other skills in a real situation.

---

## When This Skill Applies

- User says "set up Zoom Suite" or "zoom setup" or "zoom onboarding"
- User installs the Zoom Suite plugin for the first time
- User says "configure zoom" or "get zoom ready"
- User says "test my zoom connection"
- **system-04 activating this plugin** — always run this skill first at activation

---

## Phase 1: Preflight Checks

Run all three checks silently in sequence. Report results together after all three complete.

---

### Check 1 — Zoom MCP Connectivity

Call `mcp__zoom__recordings_list` with:
- `from`: 30 days ago
- `to`: today
- `page_size`: 1

**Pass:** Returns a result (even an empty list). The connector is live and authenticated.

**Fail:** Returns an authentication error, timeout, or tool-not-found error.

→ If fail: Stop and report:
```
❌ Zoom MCP connector not responding.

The Zoom Suite plugin requires the Zoom MCP connector to be installed and connected in Cowork.

To fix this:
1. Open Cowork Settings → Connectors
2. Find "Zoom" in the available connectors
3. Click Connect and follow the OAuth flow to authorize your Zoom account
4. Return here and run "zoom setup" again

If the connector is already installed but showing as disconnected, try removing and re-adding it.
```
Do not proceed to Phase 2 until connectivity passes.

---

### Check 2 — Zoom AI Companion (Meeting Summaries)

Call `mcp__zoom__search_meetings` with:
- `from`: 14 days ago
- `to`: today
- `page_size`: 3

Take the most recent meeting UUID from the results and call `mcp__zoom__get_meeting_assets`.

Inspect the `meeting_summary` field in the response.

**Pass:** `meeting_summary` contains content (a recap, key points, or next steps).

**Partial:** `meeting_summary` is null or empty, but the meeting exists. AI Companion is not generating summaries.

**No meetings found:** Skip this check — flag as unknown and note it in the report.

→ If partial (AI Companion not active):
```
⚠️ Zoom AI Companion not detected.

The debrief and digest skills work best with Zoom AI Companion enabled — it generates
meeting summaries and next steps automatically. Without it, skills fall back to
transcripts only (which requires cloud recording).

To enable AI Companion:
1. Sign in to zoom.us → Admin → AI Companion
2. Enable "Meeting Summary" and "Meeting Questions"
3. Note: AI Companion requires a Zoom Pro, Business, or Enterprise plan.

You can continue without it — skills will use transcripts where available.
```

---

### Check 3 — Cloud Recording

Inspect the `recordings_list` result from Check 1.

**Pass:** At least one cloud recording found in the past 30 days.

**Fail:** Empty list returned (no cloud recordings).

→ If fail:
```
⚠️ No cloud recordings found in the past 30 days.

comm-zoom-digest relies on cloud recordings for its weekly summary. Without cloud
recording enabled, the digest skill will have no data to work with.

To enable Cloud Recording:
1. Sign in to zoom.us → Settings → Recording
2. Under "Cloud Recording," toggle it on
3. Check "Record active speaker with shared screen" at minimum
4. Note: Cloud Recording requires a paid Zoom plan.

The debrief and prep skills can still use AI summaries if AI Companion is active.
```

---

### Check Report

After all three checks, show a consolidated status report before proceeding:

```
─────────────────────────────────────
Zoom Suite Preflight Check
─────────────────────────────────────
✅ Zoom MCP connector       Connected
[✅/⚠️] AI Companion         [Active / Not detected]
[✅/⚠️] Cloud Recording      [Enabled / Not found]
─────────────────────────────────────
```

If all three pass: *"Everything looks good. Let's set up your preferences — this takes about 3 minutes."*

If there are warnings: *"There are some warnings above — the plugin will still work but some features will be limited. You can fix these now or come back to them later. Want to continue with setup?"*

If connectivity failed: Stop. Do not continue to Phase 2.

---

## Phase 2: Preference Collection

Collect all preferences for all four skills in a single interview. Save everything at the end — don't save piecemeal.

Tell the user: *"I'll ask you a few quick questions that cover all four skills. Answer everything in one message if you'd like — I'll sort it out."*

---

**Ask these questions as a grouped set:**

```
A few quick questions to configure your Zoom Suite:

1. Your name — how should I identify your action items in meeting debriefs?

2. Your role and company (optional — used for meeting prep research context):
   e.g., "Founder at Acme Consulting" or "Sales Director at XYZ Corp"

3. What's your timezone?
   e.g., America/Phoenix, America/New_York, America/Los_Angeles

4. For undated action items in meeting debriefs — add a default deadline?
   Options: 1 business day / 3 business days / 1 week / No default

5. After debriefing a meeting, should I automatically create a Zoom Doc with 
   the structured output? Yes / No

6. Weekly digest depth — when I summarize your meetings each week, how much detail?
   Brief (1 paragraph per meeting) or Detailed (full decisions + action items per meeting)

7. Weekly digest: save as a Zoom Doc so your team can see it? Yes / No

8. Any meeting types to EXCLUDE from your weekly digest?
   e.g., "skip my daily standup" or "skip 1:1s with my assistant" — or "none"
```

Wait for the user's response, then confirm what was captured:

```
Got it — here's what I'll use:

Name: [name]
Role/Company: [role] — [company]
Timezone: [tz]
Default deadline for undated actions: [deadline]
Auto-create Zoom Doc after debrief: [Yes/No]
Weekly digest depth: [Brief/Detailed]
Weekly digest as Zoom Doc: [Yes/No]
Meeting exclusions: [exclusions or "none"]

Saving now...
```

---

**Save preferences to Cloud Brain in three files:**

**Debrief + general preferences:**
- Tool: `mcp__cloud-brain__write_note`
- Path: `brain/preferences/zoom-debrief-preferences.md`
- Content:
  ```markdown
  # Zoom Debrief Preferences
  
  - Name: [name]
  - Timezone: [timezone]
  - Default deadline for undated action items: [deadline or "none"]
  - Auto-create Zoom Doc after debrief: [Yes/No]
  ```

**Meeting prep preferences:**
- Tool: `mcp__cloud-brain__write_note`
- Path: `brain/preferences/meeting-prep-preferences.md`
- Content:
  ```markdown
  # Meeting Prep Preferences
  
  - Name: [name]
  - Role: [role]
  - Company: [company]
  - Timezone: [timezone]
  - Primary meeting goal: [collected or "not specified"]
  ```

**Digest preferences:**
- Tool: `mcp__cloud-brain__write_note`
- Path: `brain/preferences/zoom-digest-preferences.md`
- Content:
  ```markdown
  # Zoom Digest Preferences
  
  - Name: [name]
  - Timezone: [timezone]
  - Digest time window: Past 7 days (Mon–Sun)
  - Digest depth: [Brief/Detailed]
  - Save as Zoom Doc: [Yes/No]
  - Excluded meeting types: [exclusions or "none"]
  ```

Confirm: *"Preferences saved. All four skills will load these automatically from now on."*

---

## Phase 3: Live Test

Tell the user: *"Last step — I'll run a live debrief on your most recent recorded meeting so you can see the plugin working."*

From the `recordings_list` result in Phase 1, take the most recent meeting.

If no recordings were found (Check 3 failed): Skip Phase 3 and go to Phase 4. Note: *"Skipping the live test — no cloud recordings available yet. Once Cloud Recording is enabled and you've completed a meeting, run 'debrief my last meeting' to see the skill in action."*

If recordings exist:
1. Call `mcp__zoom__get_meeting_assets` on the most recent recording's UUID
2. Call `mcp__zoom__get_recording_resource` with `types: "transcript,summary,nextStep"` if available
3. Run the full debrief extraction (decisions, action items, follow-ups, key insights) as defined in `comm-zoom-debrief`
4. Present the debrief output to the user

After showing the debrief output, ask:
*"Does this look right? If the meeting wasn't a useful test (e.g., it was a short internal call), say the name or topic of a better one and I'll run the test on that instead."*

---

## Phase 4: Setup Complete

Deliver a final summary and next steps:

```
─────────────────────────────────────
Zoom Suite Setup Complete ✅
─────────────────────────────────────
Connector:        Connected
AI Companion:     [Active / ⚠️ Not detected — see notes above]
Cloud Recording:  [Enabled / ⚠️ Not found — see notes above]

Preferences saved for:
  ✅ comm-zoom-debrief
  ✅ comm-zoom-prep
  ✅ comm-zoom-digest
  ✅ comm-zoom-search (no preferences needed)

─────────────────────────────────────
What to try next:

"Debrief my last meeting"
  → Pulls and structures any Zoom meeting automatically

"Prep me for my call with [name]"
  → Pulls your history + builds a full prep brief

"What did we decide about [topic]?"
  → Searches all your meetings, chat, and docs

"Weekly meeting digest"
  → Or schedule it: "Run my zoom digest every Monday at 7am"
─────────────────────────────────────
```

If there were any ⚠️ warnings during preflight, add:
*"When you've fixed the settings flagged above, run 'zoom setup' again — it will recheck and update your status without re-collecting preferences."*

---

## Re-Run Behavior

If preferences already exist in Cloud Brain when this skill is run again:

*"Your Zoom Suite preferences are already set up. I'll run the preflight checks to verify your connection status, then show you your current preferences. Do you want to update any settings, or just run the connectivity check?"*

- **Connectivity check only:** Run Phases 1 and 4 only. Skip preference collection and live test.
- **Update preferences:** Re-run Phase 2. Pre-fill answers with saved values so the user only has to change what's different.
- **Full re-run:** Run all phases.

---

## Error Handling

- **Zoom MCP not found / not connected:** Stop at Check 1. Provide install instructions. Do not proceed.
- **No meetings in the past 14 days (Check 2):** Skip AI Companion check. Note: "No recent meetings found to test AI Companion — this check will run automatically the next time you debrief a meeting."
- **User skips Phase 3:** Respect it. Move to Phase 4 without the test.
- **User answers preferences partially:** Fill in defaults for anything not provided, confirm with the user, then save.
- **Brain save fails:** Report the error and tell the user they can run the skill again. Do not leave preferences in a partial state — either all three files save or none do (retry once before reporting failure).
