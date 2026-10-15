---
name: prospect-icp-define
description: >-
  Define and save your Ideal Customer Profile (ICP) — industry, company size, title, geography, tech stack, buying triggers, and disqualifiers. ICP is the foundation every other prospecting skill reads from, so every list, search, and enrichment is calibrated to who you actually want to talk to. Supports multiple ICPs (e.g., one for your agency clients, one for real estate sellers, one for podcast guests).
---

# ICP Define

## Overview

Set up the buyer profile that every other prospecting skill — `prospect-find`, `prospect-enrich`, `prospect-bulk-list`, `prospect-handoff` — will read from. Multiple ICPs are supported, each saved as a separate note under `brain/icp/` in Cloud Brain. Skills that need an ICP either accept `--icp <name>` or default to the first one defined.

This is the prospecting equivalent of the `investor-profile` plugin's `buy-box` skill — same idea, applied to B2B sales.

## When This Skill Applies

- User wants to define who they sell to ("define my ICP", "set up my buyer profile")
- User wants to update or view their ICP ("update my ICP", "show me my ICPs")
- User is starting prospecting for the first time and no ICP exists yet
- Another prospecting skill (find, enrich, bulk-list) needs an ICP and none is defined → it should prompt the user to run this skill first
- User says: "ideal customer profile", "buyer profile", "who do I target"

## Pre-Flight — Preferences

This skill IS the preferences for the prospecting plugin. There's no upstream preference to load — but check whether ICPs already exist:

1. `list_directory` → folder `brain/icp/`
2. **If ICPs exist:** Show the list with names, summary lines, last-updated dates. Ask: "Add a new ICP, update one, or just view? (new / update / view)"
3. **If none exist:** Proceed straight to the define flow.

## How It Works

### Step 1: Gather ICP Details

Ask in ONE message (don't drip the questions one at a time):

| Field | Examples |
|-------|----------|
| ICP nickname | `agency-clients`, `re-sellers-utah`, `podcast-guests-real-estate` |
| Industries (one or more) | "SaaS", "Real estate brokerages", "Med spas in Utah" |
| Job titles (one or more) | "Founder", "CMO", "Head of Growth" |
| Company size | "1-10 employees", "11-50", "51-200", "200+", or "any" |
| Annual revenue range | "$1M-$10M", or "any" |
| Geography | "US only", "Utah + Indiana", "English-speaking", "global" |
| Tech stack signals | "uses HubSpot", "WordPress site", "has a Calendly link" |
| Buying triggers | "recently raised", "hired a VP of Sales", "launched a podcast" |
| Disqualifiers | "competitors", "Big 4 consulting", "agencies (if you ARE one)" |
| What they need from you | 1-2 sentences — the pain you solve |
| Avg deal size | "$5K-$25K", "$50K+", "TBD" |

Optional advanced fields (offer but don't require):
- Personas within the company (champion vs. economic buyer vs. influencer)
- Preferred outreach channel (email / LinkedIn / phone / mail)
- Anti-personas — people NOT to talk to even in target companies

### Step 2: Save the ICP

Use `write_note`:
- **title:** `ICP — {nickname}`
- **folder:** `brain/icp`
- **tags:** `["icp", "{nickname}"]`

Use the template in Data Structure.

### Step 3: Confirm and Cross-Link

After saving:
- Render a clean summary of the ICP
- Show next-step suggestions:
  - "Run `/prospect-find --icp {nickname}` to start finding matches"
  - "Run `/prospect-bulk-list --icp {nickname} --count 50` to generate a starting list"

If this is the user's first ICP, also update `brain/preferences/prospecting-preferences` (create if missing) to mark this as the default ICP.

## Data Structure

```markdown
# ICP — {nickname}

> **Created:** {YYYY-MM-DD}
> **Updated:** {YYYY-MM-DD}
> **Default ICP:** {yes / no}

## Target Profile

| Field | Value |
|-------|-------|
| Industries | {comma list} |
| Job Titles | {comma list} |
| Company Size | {range} |
| Revenue Range | {range or "any"} |
| Geography | {regions} |
| Tech Stack Signals | {tools / signals} |
| Buying Triggers | {events that suggest readiness} |
| Disqualifiers | {who NOT to target} |

## What They Need

{1-2 sentence pain description and how you solve it}

## Deal Economics

| Field | Value |
|-------|-------|
| Avg Deal Size | ${range} |
| Sales Cycle | {days / weeks / months} |
| Decision Maker | {title} |
| Influencers | {titles} |

## Personas (Optional)

### Champion
- {role / pain / what they want}

### Economic Buyer
- {role / pain / what they want}

### Influencer
- {role / pain / what they want}

## Outreach Preferences

- Preferred channel: {email / LinkedIn / phone / mail}
- Best time to reach: {time of day / day of week if known}
- Anti-personas: {who in target companies NOT to message}

## Notes

- {YYYY-MM-DD}: ICP created.
```

## Output Format

```
✓ ICP saved: {nickname}

INDUSTRIES: {list}
TITLES: {list}
COMPANY SIZE: {range}
GEO: {regions}
DEAL SIZE: ${range}

NEXT STEPS
1. Find matching contacts: /prospect-find --icp {nickname}
2. Generate a target list:  /prospect-bulk-list --icp {nickname} --count 50
3. Enrich a known contact:  /prospect-enrich [name] --company [company]
```

## Example Usage

**User:** "Define my ICP"

**AI:** Lists existing ICPs (if any). Asks the 11-field ICP question in ONE message. Saves to Cloud Brain. Shows next steps.

**User:** "Set up an ICP for my agency — SMBs that need AI automation"

**AI:** Pre-fills industries (SMBs, professional services), titles (Founder, COO), suggests buying triggers (recent funding, new hires, "AI" in social posts). User confirms or edits. Saves.

**User:** "/prospect-icp-define re-sellers-utah --industry 'real estate, single-family homes' --title 'Homeowner, Landlord' --geo 'Utah' --buying-triggers 'expired listing, recent divorce, probate, tax lien'"

**AI:** Creates `ICP — re-sellers-utah` immediately from arguments. Asks for any missing fields. Saves.

**User:** "Show my ICPs"

**AI:** Lists all `brain/icp/` notes with name, industries, titles, last-updated dates. Asks which to view in full.

**User:** "Update my agency-clients ICP to add 'launched a podcast in last 90 days' as a buying trigger"

**AI:** Reads existing ICP, appends to buying triggers, saves.

## Error Handling

- **If user names an ICP that conflicts with existing one:** Confirm "An ICP called {name} already exists. Replace it, update it, or pick a different name?"
- **If user provides too-vague fields (e.g., "anyone", "all")**: Ask for one more constraint. ICPs that match everything match nothing.
- **If user says "I don't know yet" for several fields:** Save what they have, mark missing fields as `TBD`, and flag: "This ICP has gaps. Other prospecting skills will work but will produce broader lists. Fill in `TBD` fields when you have more data."
- **If geography is given but no region/state is named:** Ask "Specific countries, states, or metros? 'US' is fine; 'global' is fine; 'undefined' won't work for list generation."
- **If user already has 5+ ICPs:** Ask "You have {N} ICPs. Are these all active, or should we archive some? Too many ICPs usually means none are tight enough."
- **If the user's prior ICP is RE-specific and they have `investor-profile` installed:** Note: "You have a real-estate buy box in `investor-profile`. Want this ICP to extend that, or keep them separate? Same fields can stay in sync."

## See Also

- `/prospect-find` — find matches for this ICP (same plugin)
- `/prospect-bulk-list` — generate a target list from this ICP (same plugin)
- `/realestate-buy-box` — the RE-specific cousin of ICP (from `investor-profile`)
