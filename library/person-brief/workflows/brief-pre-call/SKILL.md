---
name: brief-pre-call
description: >-
  Sales-call-specific pre-call brief. Pulls CRM context (deal stage, value, last touch) from your pipeline, merges with last 30 days of prospect activity, and generates a call-specific game plan — opener, discovery questions tied to recent signal, anticipated objections, recommended next step. Designed to run 5-15 minutes before every sales call.
---

# Pre-Call Brief

## Overview

The sales-call counterpart to **brief-pre-meeting**. Where pre-meeting is general-purpose, pre-call is purpose-built for deals — it assumes a known deal stage, generates discovery questions tied to the prospect's recent public activity, anticipates objections, and recommends a call-purpose-specific next step (book demo / send proposal / handle objection / ask for close).

Hard-wires into `bizops-lead-tracker` and `bizops-pipeline-sync` if installed.

## When This Skill Applies

- User has a sales call scheduled and asks for prep
- User says "prep me for the {Name} call", "pre-call brief", "what should I cover with {Name}"
- User mentions a prospect name and call type (discovery / demo / negotiation / close)
- User is about to dial a number or join a sales call
- Pipeline review surfaces a stale deal that needs attention before the next touch

## Pre-Flight — Preferences

1. Search Cloud Brain: `search_notes` with query `"pre-call preferences"` (folder: `brain/preferences`)
2. **If found:** Load, render banner, proceed
3. **If not found:** Ask in ONE message:
   - Default sales methodology (Sandler / SPIN / Challenger / consultative / freeform)
   - Default call duration (15 / 30 / 45 / 60 minutes)
   - Your offering — 1-2 sentences for context (what you sell, who it's for)
   - Common objection categories you face (price / timing / authority / need / fit)
   - Save to Cloud Brain: `write_note` → title: `pre-call-preferences`, folder: `brain/preferences`
4. Banner:
   ```
   🎯 Pre-Call | Methodology: {method} | Duration: {min} | Offering: ✓ loaded
   ```

## How It Works

### Step 1: Resolve Prospect + Deal Context

1. Search `pipeline/` for prospect → pull deal stage, value, last interaction, notes
2. Search `people/` for contact note → pull background, family, preferences
3. Search `transcripts/` and `calendar/` for prior call notes
4. If no CRM record: create one in `bizops-lead-tracker` (if installed) or note as "new prospect, no CRM history"

### Step 2: Determine Call Purpose

If user didn't specify, infer from deal stage:
- `new` / `contacted` → discovery
- `qualified` → demo or scoping call
- `negotiating` → objection handling or close
- `closed-lost` → win-back conversation

Confirm with user if ambiguous: "Is this a discovery call or are we moving to demo?"

### Step 3: Pull 30-Day Public Activity

Invoke **brief-person-30days** (or inline source sweep) for the prospect. Focus on signals relevant to sales:
- Pain points they've publicly shared
- Competitors they've mentioned or are using
- Recent role changes / company news
- Topics they're engaging with that connect to your offering

### Step 4: Generate Call Game Plan

Based on call purpose and signal, generate:

**Discovery call:**
- 3 discovery questions tied to recent signal
- 2 questions tied to deal stage / their stated needs
- 1 disqualification question

**Demo call:**
- Demo flow tailored to their recent posts / pain points
- 2-3 "wow" moments to anchor on
- Anticipated questions during demo

**Negotiation:**
- Anticipated objections (top 3) with prepared responses
- BATNA reminder
- Concessions you're willing to make (ranked)

**Close:**
- Recap of value delivered to this point
- Clear ask
- Anticipated final objections + responses

### Step 5: Save and Render

- `write_note` → title: `Pre-Call Brief — {Prospect Name} — {YYYY-MM-DD}`, folder: `calendar`, tags: `["pre-call", "sales", "{deal-stage}"]`
- Render condensed version in chat
- Append to prospect's lead note in `pipeline/` (Interaction History entry): "Pre-call brief generated {date}"

## Data Structure

```markdown
# Pre-Call Brief — {Prospect Name}

> **Call Time:** {YYYY-MM-DD HH:MM}
> **Call Purpose:** {discovery / demo / negotiation / close}
> **Deal Stage:** {stage} • **Value:** ${amount} • **Probability:** {%}
> **Methodology:** {Sandler / SPIN / Challenger / etc.}

## Prospect Snapshot

| Field | Value |
|-------|-------|
| Name | {name} |
| Title / Company | {role at company} |
| Decision Authority | {yes / influencer / unknown} |
| Last Touch | {date — channel — summary} |
| Days in Stage | {N} |

## Recent Signal (Last 30 Days)

- {Theme} — {1-line + 1 link}
- {Theme} — {1-line + 1 link}

## Shared Context

- **Their stated need:** {what they've told you they want}
- **Pain points (public + private):** {merge of CRM notes + recent posts}
- **Their tools / competitors:** {what they currently use or have mentioned}

## Call Game Plan

### Opener (30 seconds)
{1-2 sentences that reference recent signal AND last interaction}

### Discovery Questions (or Demo Flow / Objection Handling)

**Tied to recent signal:**
1. {question — references something they posted/said publicly}
2. {question}

**Tied to deal stage:**
3. {question — moves the deal forward}
4. {question}

**Disqualifier:**
5. {question that surfaces a deal-killer fast}

### Anticipated Objections

| Objection | Response |
|-----------|----------|
| {objection 1} | {1-2 sentence response} |
| {objection 2} | {response} |
| {objection 3} | {response} |

### The Ask

{Specific next step you want them to commit to by end of call}

### If They Say No to the Ask

{Fallback ask — smaller commitment that still moves the deal}

## Don't Forget

- {personal detail from contact note}
- {commitment from last call that you owe them}
- {timing constraint they mentioned}

## After the Call

- [ ] Update pipeline stage if changed
- [ ] Log call notes via `comm-meeting-actions`
- [ ] Send follow-up within 24h
- [ ] Update next follow-up date
```

## Output Format (Chat)

```
🎯 PRE-CALL BRIEF — {Prospect}
{Call time} • {purpose} • Stage: {stage} • $${value}

LAST TOUCH: {date} — {channel} — {1-line}
DAYS IN STAGE: {N}

RECENT SIGNAL (last 30d)
• {theme}
• {theme}

OPENER
"{30-second opener}"

TOP 3 DISCOVERY QUESTIONS
1. {question}
2. {question}
3. {question}

ANTICIPATED OBJECTIONS
• {obj} → {response}
• {obj} → {response}

THE ASK: {next step}

Full brief: calendar/Pre-Call Brief — {name} — {date}
```

## Example Usage

**User:** "Prep me for my 2pm with John Smith at ABC Motors. It's a demo."

**AI:** Pulls John Smith from `pipeline/` — qualified stage, $5K deal, last touch 4 days ago via email. Runs 30-day brief — John has been posting about inventory management pain. Generates demo flow that anchors on inventory pain point. Saves brief.

**User:** "/brief-pre-call Sarah Lee --call-purpose close --company Acme"

**AI:** Pulls Sarah from pipeline (negotiating, $15K). Recent signal: she posted about budget approvals on LinkedIn. Generates close-stage objection handling, BATNA reminder, and primary ask. Saves brief.

**User:** "I have a discovery call in 10 minutes with Marcus Thompson from Lakefront Realty"

**AI:** Detects Marcus is not in pipeline. Runs quick brief on Marcus + Lakefront Realty (lighter depth due to time crunch). Creates new lead in `bizops-lead-tracker`. Generates 5 discovery questions. Renders condensed brief immediately.

## Error Handling

- **If prospect is not in CRM and lead-tracker is installed:** Offer to create a new lead before generating the brief. If user says yes, create with stage `new`, source "Pre-call brief", and proceed.
- **If prospect is not in CRM and lead-tracker is NOT installed:** Generate the brief anyway, but tag the note with `["no-crm"]` and recommend: "You don't have lead-tracker installed — install `business-operations` plugin to track this deal."
- **If call is in <5 minutes:** Skip 30-day brief entirely. Generate only opener + top 3 discovery questions + top 1 anticipated objection. Render in chat — don't save to Cloud Brain (too rushed).
- **If user didn't specify call purpose and stage is ambiguous:** Ask once. Default to discovery if no answer.
- **If user has multiple deals with same prospect name:** Disambiguate by company / deal value. Don't merge.
- **If deal is in `closed-lost`:** Confirm: "This deal is marked closed-lost. Are you running a win-back call?" Generate a win-back-specific brief if yes.
- **If prospect's recent signal is genuinely empty:** Lean entirely on CRM history. Note: "No public signal in 30 days — relying on your prior conversations."
- **If the user's offering / preferences aren't set:** Run the pre-flight ask. Cannot generate a useful objection list without knowing what you sell.

## See Also

- `/brief-pre-meeting` — non-sales meeting prep (same plugin)
- `/brief-person-30days` — research the prospect without sales framing (same plugin)
- `/bizops-lead-tracker` — track the deal through pipeline (from `business-operations`)
- `/comm-negotiation-prep` — deeper objection-handling and BATNA work (from `communications`)
- `/prospect-enrich` — enrich the prospect's contact data before the call (from `prospecting`)
