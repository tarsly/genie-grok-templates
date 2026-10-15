---
name: eos-vto-builder
description: >-
  Build or refresh your EOS V/TO (Vision/Traction Organizer) — the 2-page document that captures Core Values, Core Focus, 10-Year Target, Marketing Strategy, 3-Year Picture, 1-Year Plan, Quarterly Rocks, and Issues. Use for vision sessions, annual planning, quarterly V/TO review, core values discovery, 3-year picture drafts, 10-year BHAG, marketing strategy (Three Uniques, Proven Process, Guarantee), or needs to build their V/TO, update a section, or run a quarterly vision review.
---

# V/TO Builder — Your Vision in 8 Questions

## Overview

The V/TO (Vision/Traction Organizer) is the foundation of EOS. Two pages. Eight questions. Every leader on the team must be able to answer the same way. This skill walks an owner through the full V/TO — drafting Core Values via the 3-Lists Exercise, defining Core Focus (Purpose + Niche), setting the 10-Year Target, building the Marketing Strategy (Target Market, Three Uniques, Proven Process, Guarantee), painting the 3-Year Picture, sketching the 1-Year Plan, and linking to current Quarterly Rocks + the long-term Issues List. Saves a versioned snapshot to Cloud Brain so every Quarterly and Annual meeting builds on it.

## When This Skill Applies

- User wants to build their V/TO for the first time
- User wants to refresh part of an existing V/TO (typical at Quarterly or Annual Planning)
- User asks about Core Values, Core Focus, 10-Year Target, BHAG, Marketing Strategy, Three Uniques, Proven Process, Guarantee, 3-Year Picture, or 1-Year Plan
- User says "build my V/TO", "update vision", "core values exercise", "3-year picture", "10-year target"
- User asks "what is our vision" or "show our V/TO"
- The `eos-quarterly-pulsing` or `eos-annual-planning` skill needs the Vision side reviewed
- User invokes `/eos-vto-builder`

## Pre-Flight — Onboarding + Preferences

Before asking anything, run these searches in parallel:

1. `mcp__cloud-brain__search_notes` query `business blueprint` — extract company name, industry, team size, pain points
2. `mcp__cloud-brain__search_notes` query `goal-hierarchy` — extract keystone goal + long-horizon vision (seeds 10-Year Target + 3-Year Picture)
3. `mcp__cloud-brain__search_notes` query `quarterly-priorities` — current quarter commitments
4. `mcp__cloud-brain__search_notes` query `language-and-frameworks` — whether the user prefers EOS terminology
5. `mcp__cloud-brain__search_notes` query `eos company preferences` (folder `brain/preferences`) — saved EOS prefs
6. `mcp__cloud-brain__search_notes` query `EOS V/TO` — existing V/TO snapshot

**Reconcile:**
- If `eos-company-preferences` exists, load it.
- Else if `business blueprint` exists, pre-fill prefs from it and confirm in ONE message: *"I see you're [Company] in [Industry], team of [N]. Using that for your V/TO — sound right?"*. Save to `eos-company-preferences` on confirmation.
- Else ask in ONE message: company name, industry, team size, fiscal year start, leadership roster. Save.

**Banner** (top of every output):
```
🎯 EOS V/TO Builder | {Company Name} | {Quarter} {Year} | Onboarding: ✓ loaded
```
Add: *"Say 'update my EOS preferences' to change company settings."*

## How It Works

### Step 0: Determine the Mode

Parse the user's request into one of:
- **`build`** — first-time V/TO from scratch (or no existing V/TO found)
- **`refresh`** — update an existing V/TO (default if one exists)
- **`view`** — show the current V/TO without editing
- **`section <name>`** — work only one section: `core-values`, `core-focus`, `10-year`, `marketing`, `3-year`, `1-year`, `rocks`, `issues`

If no V/TO exists, default to `build`. If one exists, default to `view` and ask: *"Want to refresh anything? Pick a section or say 'refresh all'."*

### Step 1: Build Mode — Walk the 8 Questions

Work through each section in order. Save progress after each section so the user can pause and resume.

#### Question 1 — Core Values (3-7, ideally 5)

Use the **3-Lists Exercise**:

1. Ask: *"Name 3 people on your team (or 3 you'd clone) who embody what your culture should be."*
2. Ask: *"List the traits that make those people great — go broad, 30-50 raw traits is normal."*
3. Group similar traits, then circle the 3-7 that define your culture. Reject aspirational fluff — these describe people who actually succeed here.

For each value, capture:
- One-sentence definition
- 2-3 behavioral examples ("looks like…")
- 1 anti-example ("does not look like…")

Save to a separate note `EOS Core Values` in the `eos` folder so `eos-people-analyzer` can read it.

#### Question 2 — Core Focus

Two parts, each one sentence:
- **Purpose / Cause / Passion** — Why you exist (e.g., *"To help small business owners get what they want from their business."*)
- **Niche** — What you do (e.g., *"Helping entrepreneurs run on EOS."*)

If the user has a `goal-hierarchy` note, seed the Purpose from their keystone goal.

#### Question 3 — 10-Year Target (BHAG)

One audacious, measurable 10-year goal. Drawn from Jim Collins's BHAG concept. Examples: *"$100M in revenue by 2036," "Open in 25 cities," "Become the #1 [category] in the country."*

If `goal-hierarchy` exists, propose the Level 1 vision as a starting point.

#### Question 4 — Marketing Strategy

Four parts:
- **Target Market / "The List"** — Demographic + Geographic + Psychographic profile. Ideally a named prospect list.
- **Three Uniques** — Three things that *in combination* make you different. Classic example: Domino's = (1) hot fresh pizza (2) delivered (3) in 30 min or less.
- **Proven Process** — A 3-7 step named, visualized process every client experiences. Propose a name (e.g., "The [Company] Way").
- **Guarantee** — Risk-reversal addressing the prospect's biggest fear (optional but recommended).

#### Question 5 — 3-Year Picture

Future date snapshot — written in present tense as if you're there.
- Future date
- Future revenue + profit
- Future measurables (head count, locations, etc.)
- 5-15 bullet list of "What does it look/feel like?" — *"100 employees," "Operating in 3 cities," "All leaders running L10s."*

#### Question 6 — 1-Year Plan

Future date = end of current fiscal year (use prefs).
- Revenue target
- Profit target
- 3-7 SMART goals

#### Question 7 — Quarterly Rocks

Reference only — `eos-rocks` owns this. Read `EOS Rocks — {current quarter}` and display the list.

#### Question 8 — Issues (long-term)

Reference only — `eos-issues` owns this. Read `EOS Issues List` and display the **long-term** section.

### Step 2: Refresh Mode

Read `EOS V/TO` via `mcp__cloud-brain__read_note`. Show the current values. Ask which sections to revisit. Walk only those. Save changes via `mcp__cloud-brain__edit_note`.

Before overwriting, archive the current V/TO to `EOS V/TO Archive — YYYY-MM-DD` via `mcp__cloud-brain__write_note` (tags `["eos","vto","archive"]`).

### Step 3: View Mode

`read_note` on `EOS V/TO` and render the two-column V/TO layout (see Output Format).

### Step 4: Save

`mcp__cloud-brain__write_note` (or `edit_note` if exists):
- **title:** `EOS V/TO`
- **folder:** `eos`
- **tags:** `["eos","vto"]`

Use the Data Structure template below.

### Step 5: Publish (optional)

If user passed `--publish` or said "send to dashboard":
1. `mcp__cloud-dashboard__get_agent_persona` — apply tone
2. `mcp__cloud-dashboard__publish_report` with:
   - `taskName: "eos-vto"`
   - `category: "briefing"`
   - `content`: the rendered V/TO markdown
   - `summary`: *"V/TO refreshed. Core Focus: {one-line}. 10-Year Target: {target}. 3 of 7 sections updated."*
   - `tags: ["eos","vto","vision"]`

## Data Structure

`EOS V/TO` note in folder `eos`:

```markdown
# EOS V/TO — {Company Name}

> **Last Updated:** {YYYY-MM-DD}
> **Fiscal Year:** {start month}
> **Current Quarter:** Q{N} {YYYY}

---

## VISION

### 1. Core Values
1. **{Value Name}** — {one-sentence definition}
   - Looks like: {behavior 1}; {behavior 2}; {behavior 3}
   - Not: {anti-example}
2. **{Value Name}** — …

### 2. Core Focus
- **Purpose / Cause / Passion:** {one sentence}
- **Niche:** {one sentence}

### 3. 10-Year Target
{One BHAG-style sentence with a year.}

### 4. Marketing Strategy
- **Target Market / The List:** {demographic + geographic + psychographic}
- **Three Uniques:**
  1. {unique #1}
  2. {unique #2}
  3. {unique #3}
- **Proven Process:** {name} — {3-7 named steps}
- **Guarantee:** {if any}

### 5. 3-Year Picture
- **Future Date:** {YYYY-MM-DD}
- **Revenue:** ${X}
- **Profit:** ${X}
- **Measurables:** {list}
- **What does it look like?**
  - {bullet 1}
  - {bullet 2}
  - …

---

## TRACTION

### 6. 1-Year Plan
- **Future Date:** {YYYY-MM-DD}
- **Revenue:** ${X}
- **Profit:** ${X}
- **Goals for the Year:**
  1. {SMART goal 1}
  2. {SMART goal 2}
  …

### 7. Quarterly Rocks
*See `EOS Rocks — {current quarter}` for the live list.*

### 8. Issues (Long-Term)
*See `EOS Issues List` → Long-Term section.*
```

## Output Format

### View Mode

```
🎯 EOS V/TO Builder | {Company Name} | Q{N} {YYYY} | Onboarding: ✓ loaded

╔══════════════════════════════════════╦══════════════════════════════════════╗
║              VISION                  ║             TRACTION                 ║
╠══════════════════════════════════════╬══════════════════════════════════════╣
║ Core Values: {comma-separated}       ║ 1-Year Plan: ${rev} / ${profit}     ║
║ Core Focus: {purpose} / {niche}      ║ Goals (3-7): {count} set            ║
║ 10-Year Target: {one line}           ║ Quarterly Rocks: {N} active         ║
║ Marketing: {3 uniques highlights}    ║ Issues (Long-Term): {N}             ║
║ 3-Year Picture: {revenue/year}       ║                                      ║
╚══════════════════════════════════════╩══════════════════════════════════════╝

(Full V/TO below — markdown)
```

### Build/Refresh Section Confirmation

```
✅ Section saved: Core Values
   {value 1}, {value 2}, {value 3}, {value 4}, {value 5}

Next up: Core Focus. Ready?
```

## Example Usage

**User:** "Build my V/TO"
**AI:** Loads blueprint + goal-hierarchy. Confirms company name. Walks through all 8 sections one at a time, saving after each. Renders the final V/TO. Asks: *"Publish to your cloud dashboard?"*

**User:** "Refresh our 3-Year Picture — we just opened our 2nd location."
**AI:** Reads `EOS V/TO`. Archives current snapshot. Walks only Section 5. Confirms new bullets ("2 locations operational, hiring for the 3rd"). Saves.

**User:** "/eos-vto-builder section core-values"
**AI:** Loads existing V/TO. Walks 3-Lists Exercise. Saves new Core Values to both `EOS V/TO` (Section 1) and `EOS Core Values` (standalone note). Confirms.

**User:** "Show me our V/TO"
**AI:** Reads `EOS V/TO`, renders two-column layout, lists section-by-section.

**User:** "What's our 10-year target?"
**AI:** Reads `EOS V/TO`, returns Section 3 + one-line context.

**User:** "/eos-vto-builder refresh --publish"
**AI:** Walks all 8 sections in refresh mode. After saving, publishes to dashboard with category `briefing`.

## Error Handling

- **No V/TO exists yet:** Switch silently to `build` mode. Don't error.
- **User abandons mid-build:** Save progress so far with tag `["eos","vto","in-progress"]`. On next run, offer to resume.
- **User answers "I don't know" for Core Focus:** Don't push. Suggest: *"Many leaders take 30-90 days to refine Core Focus. Want to use the 3-Lists Exercise on Core Values first and come back?"*
- **Core Values list >7 values:** Push back: *"More than 7 is a signal these aren't truly core. Which 2-3 are you willing to fire someone over? Those are your real values."*
- **User provides a "Rock" while building V/TO:** Acknowledge, then redirect: *"Sounds like a Quarterly Rock — let me capture that in `/eos-rocks` after we finish the V/TO."*
- **`mcp__cloud-dashboard__publish_report` fails:** Save the V/TO note successfully, then warn: *"V/TO saved to Cloud Brain. Dashboard publish failed — try `--publish` again later."*
- **Archive note already exists for today's date:** Append a sequence suffix: `EOS V/TO Archive — YYYY-MM-DD-2`.
- **User skips Marketing Strategy:** Allow it. Mark section as `[TBD]` in the saved note and remind: *"Marketing Strategy is one of the highest-leverage V/TO sections — come back to it before your next Quarterly."*
