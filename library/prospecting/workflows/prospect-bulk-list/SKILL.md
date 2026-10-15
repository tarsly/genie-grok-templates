---
name: prospect-bulk-list
description: >-
  Generate a large target list from your ICP, RE buy box, or a seed company list — then optionally enrich each entry. Designed for prospecting campaigns where you need 50-500 names ready for outreach. Outputs a CSV-ready Cloud Brain note with full enrichment columns.
---

# Prospect Bulk List

## Overview

The campaign-mode counterpart to `prospect-find`. Where find returns 10-25 well-fit contacts, bulk-list returns 50-500 contacts ready to be pushed into a sequencer or CRM. Bulk runs are slower and burn more provider quota — this skill is opinionated about budget guards, dedup, and quality gates.

Three modes:
1. **From ICP** — pulls matching contacts via provider, scored against ICP
2. **From RE buy box** — pulls property owners or sellers matching `investor-profile` buy box (uses county records, Skip Genie, web fallback if no RE-specific MCP)
3. **From seed companies** — given a list of target companies, finds N contacts per company matching titles in ICP

## When This Skill Applies

- User wants a campaign-sized list ("give me 200 prospects", "build a list for my Q3 outbound campaign")
- User has a target company list and wants contacts inside each at scale
- RE investor wants a seller list matching their buy box across a market
- User says: "build a list", "bulk list", "campaign list", "prospect list of 100+", "give me a target list"

## Pre-Flight — Preferences

Reads `prospecting-preferences`:
- Available providers
- Daily quota / monthly quota per provider (to throttle)
- Default verified-email policy
- Default dedup policy (vs. existing pipeline, vs. prior bulk lists)

If not set, ask:
- Which providers, monthly quota per provider
- Dedup against existing pipeline? (yes / no)
- Dedup against past 90 days of bulk lists? (yes / no)
- Save as CSV-ready format? (yes — useful for sequencer import)

Banner:
```
🎯 Bulk List | Providers: {detected} | Dedup: {policy} | Quota remaining: ~{N}
```

## How It Works

### Step 1: Pick Mode

Inputs determine mode:
- `--icp` → ICP mode
- `--buy-box` (or RE context) → Buy-box mode (requires `investor-profile` plugin's buy box note)
- `--seed-companies` → Seed-companies mode

If user passes none, default to ICP mode using their default ICP.

### Step 2: Plan the Run

Show the run plan BEFORE executing. This is a guardrail because bulk runs cost real money / quota:

```
RUN PLAN
========
Mode: {ICP / buy-box / seed-companies}
Source: {ICP nickname or buy-box source or company list}
Target count: {N}
Provider: {provider}
Estimated provider calls: {N}
Estimated time: {minutes}
Estimated cost: ~${X} (if pricing data is available)
Quota remaining after run: ~{Y}

Dedup: against {pipeline / prior bulk lists}
Enrich: {full / basic / off}
Verify email: {on / off}

Continue? (yes / no / adjust)
```

Don't proceed without explicit confirmation. Default to "no" if ambiguous.

### Step 3: Execute in Batches

For ICP and seed-company modes:
- Batch requests in chunks of 25-50 to provider
- Stream results — render partial batches so the user sees progress
- Dedup as you go against pipeline and prior bulk lists
- Stop if quota would be exceeded or if user interrupts

For buy-box mode (RE-specific):
- Pull from county records or RE data sources (if user has Roor/Mojo/Skip Genie MCPs)
- If `real-estate-investor` plugin is installed (detected via Cloud Brain `brain/preferences/re-investor-preferences` note), apply its creative-finance scoring. Otherwise skip the scoring layer and proceed with ICP-only ranking.
- Per-property: try to find owner name → run enrichment for contact info

### Step 4: Score, Rank, and Trim

Apply the same fit scoring as `prospect-find`. Bulk lists usually want a wider net — default min-fit threshold is 40 (vs. 50 for `prospect-find`).

If the requested count exceeds matches, return what's available and note the gap.

### Step 5: Save List

- **title:** `Bulk List — {ICP/buy-box} — {YYYY-MM-DD}` (suffix with count, e.g., `(250)`)
- **folder:** `pipeline/prospects`
- **tags:** `["bulk-list", "{source-id}", "{provider}"]`

Use the CSV-ready data structure (see below) so users can copy/paste into a sequencer.

### Step 6: Render Summary

Inline summary only (full list saved to Cloud Brain). Show:
- Counts (returned, deduped, kept)
- Fit score distribution
- Top 10 prospects (preview)
- Cost / quota burned

## Data Structure

```markdown
# Bulk List — {source} — {YYYY-MM-DD} ({count})

> **Generated:** {YYYY-MM-DD HH:MM}
> **Source:** {ICP nickname / buy-box / seed-companies}
> **Provider:** {provider}
> **Count:** {N} kept ({M} returned, {D} deduped, {L} below fit threshold)
> **Provider calls:** {N}
> **Est. cost:** ${X}

## Fit Distribution

| Bucket | Count |
|--------|-------|
| 80-100 | {N} |
| 60-79  | {N} |
| 40-59  | {N} |

## Top 10 (Preview)

| # | Name | Title | Company | Email | Phone | Fit |
|---|------|-------|---------|-------|-------|-----|
| 1 | ... |

## Full List (CSV-ready)

```csv
name,title,company,domain,email,email_status,phone,linkedin,city,fit_score,source,signals
{name},{title},{company},{domain},{email},{verified|catch-all|best-effort},{phone},{url},{city},{N},{provider},{signals}
...
```

## Notes

- {YYYY-MM-DD}: Generated from {source}. {comments}
- Dedup: skipped {N} already-in-pipeline contacts. Skipped {M} from prior bulk lists.
```

## Output Format (Chat)

```
✓ BULK LIST GENERATED
Source: {source} • Provider: {provider} • {count} contacts kept

FIT
80-100: {N}
60-79:  {N}
40-59:  {N}

QUOTA
{used} / {monthly limit} • Est. cost: ${X}

TOP 5 (preview)
1. {Name} — {title} @ {company} — fit {N}
2. ...

Full list saved: pipeline/prospects/Bulk List — {source} — {date} ({count})

NEXT
- Push high-fit (80+) into pipeline: /prospect-handoff --list "{note}" --min-fit 80
- Generate outreach: /comm-email-drafter --list "{note}"
```

## Example Usage

**User:** "Build me a 200-contact list from my agency-clients ICP, enriched and verified"

**AI:** Loads ICP. Detects Apollo. Shows run plan: 200 target, ~$X cost, 50 min estimated. Asks confirm. Executes in batches. Saves as `Bulk List — agency-clients — 2026-06-20 (200)`. Shows fit distribution.

**User:** "/prospect-bulk-list --seed-companies 'Acme,BetaCorp,GammaCo,Delta,Epsilon' --count 5"

**AI:** 5 contacts per company × 5 companies = 25 target. Shows run plan, executes, saves.

**User:** "Pull all expired listings in Salt Lake County matching my single-family buy box, give me 100 with owner contact info"

**AI:** Buy-box mode. Pulls expired listings (web fallback if no RE MCP). Cross-references with `investor-profile` buy box. For each, finds owner. Enriches. Returns 100 with contact info (or fewer if data is sparse).

**User:** "Refresh last week's bulk list — re-verify emails and add any new buying signals"

**AI:** Reads the prior bulk list. Re-runs verification on existing emails. Saves as a new note with `(refresh)` suffix.

## Error Handling

- **If user requests count exceeds remaining quota:** Show what's possible, ask whether to proceed at lower count or wait until quota resets.
- **If estimated cost is high (>$50):** Surface the cost prominently before confirmation. Don't bury it.
- **If no ICP / no buy box / no seed companies:** Refuse to proceed. Ask which mode and the seed.
- **If buy-box mode is requested without `investor-profile` plugin installed:** Suggest installing it, or proceed with a generic ICP-style search.
- **If dedup against pipeline removes >50% of results:** Note prominently: "Most matches were already in your pipeline. You may have over-fished this ICP — consider widening or moving to a new ICP."
- **If provider rate-limits mid-run:** Save partial results, note where the run stopped. Resume via `--resume` (preferred) or rerun with smaller count.
- **If returned data quality is poor (e.g., >40% emails unverified):** Flag in the summary: "Quality warning: only X% verified emails. Consider switching provider or widening ICP titles."
- **If the same person appears under two providers with different titles/companies:** Use the most recent source, mark the conflict in notes.
- **If the user has `comm-email-drafter` and wants to draft sequences for the whole list:** Suggest they sample 5 first, draft, then scale to whole list. Don't auto-draft 200 emails.

## See Also

- `/prospect-find` — smaller, higher-fit lists (same plugin)
- `/prospect-handoff` — push the list into your CRM pipeline (same plugin)
- `/realestate-buy-box` — RE-specific buy-box mode reads this (from `investor-profile`)
- `/comm-email-drafter` — draft outreach for the list (from `communications`)
