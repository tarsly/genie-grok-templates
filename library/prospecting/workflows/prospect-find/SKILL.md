---
name: prospect-find
description: >-
  Find B2B contacts matching your ICP. Provider-agnostic — uses Apollo, Hunter, Lusha, ZoomInfo, or Vibe Prospecting MCPs if installed; falls back to web search + LinkedIn/Crunchbase scrape when no provider is configured. Returns a ranked list with names, titles, companies, verified emails (where available), and a fit score against your ICP.
---

# Prospect Find

## Overview

Returns a ranked list of B2B contacts matching your ICP. The provider abstraction layer lets one skill work across whichever enrichment MCP you've installed:

1. **Apollo MCP** — best for contacts + sequences + buying signals
2. **Hunter MCP** — best for email finding + verification
3. **Lusha MCP** — best for verified mobile + lookalike
4. **ZoomInfo MCP** — best for enterprise targeting
5. **Vibe Prospecting MCP** — best for live B2B match/enrich at scale
6. **Web fallback** — LinkedIn / Crunchbase / company sites via WebSearch + WebFetch (no MCP required, lower quality)

Results are scored against the active ICP and saved to Cloud Brain as a list.

## When This Skill Applies

- User wants to find prospects ("find me 25 prospects", "who matches my ICP", "find contacts at Acme")
- User has a target company list and wants contacts inside each
- User says: "find leads", "find prospects", "find contacts", "build a prospect list"
- User is starting outbound and needs a list to email/call/DM

## Pre-Flight — Preferences

1. Search Cloud Brain: `search_notes` for `"prospecting preferences"` in `brain/preferences`
2. If found: load. Banner. Proceed.
3. If not found, ask in ONE message:
   - Which enrichment MCPs do you have installed? (Apollo / Hunter / Lusha / ZoomInfo / Vibe / none — web only)
   - Default count per search (10 / 25 / 50 / 100)
   - Auto-save lists to Cloud Brain? (yes / no)
   - Require verified email? (yes — drop anyone without verified email / no — include best-effort)
4. Detect available MCPs at runtime — if a user says they have Apollo but no `mcp__apollo__*` tools are visible, warn them.
5. Save preferences to Cloud Brain.
6. Banner:
   ```
   🎯 Prospect Find | Provider: {detected} | Default count: {N} | Verified-only: {yes/no}
   ```

## How It Works

### Step 1: Resolve the ICP

- If `--icp <name>` passed → load that ICP from `brain/icp/`
- Otherwise → load the default ICP (the one tagged `Default ICP: yes`)
- If no ICP exists → stop and direct user to `/prospect-icp-define`

### Step 2: Pick the Provider

Detect available MCPs in this priority order, unless overridden by user prefs:

1. `mcp__apollo__*` → use Apollo
2. `mcp__hunter__*` → use Hunter
3. `mcp__lusha__*` → use Lusha
4. `mcp__zoominfo__*` → use ZoomInfo
5. `mcp__vibe_prospecting__*` → use Vibe
6. None detected → web fallback (LinkedIn site search + Crunchbase + company-site careers/team pages)

Note which provider you chose in the output. If user explicitly passed `--provider <name>` and that provider isn't available, ask whether to fall back or stop.

### Step 3: Build the Query

Translate the ICP into provider-specific filters:

- **Apollo:** organizations.industry, organizations.num_employees, people.title, people.location
- **Hunter:** domain search + role + seniority
- **Lusha:** company filters + people filters + signals
- **ZoomInfo:** intent-based + filters
- **Vibe:** match/search/enrich pipeline
- **Web fallback:** `site:linkedin.com/in/ "{title}" "{industry}" "{geo}"`, `site:crunchbase.com "{industry}" "{geo}"`, plus careers page WebFetch for target companies

### Step 4: Score and Rank

For each result, score against the ICP (0-100):

- Title match → 30 points
- Industry match → 20 points
- Company size match → 15 points
- Geography match → 15 points
- Tech stack / buying trigger match → 15 points
- Verified email → 5 points

Sort descending. Apply `--count` cap. Drop below 50 unless `--include-low-fit` is passed.

### Step 5: Render and Save

Render top results inline. Save full list as a Cloud Brain note:

- **title:** `Prospect List — {ICP nickname} — {YYYY-MM-DD}`
- **folder:** `pipeline/prospects`
- **tags:** `["prospect-list", "{icp-nickname}", "{provider}"]`

Each row in the saved note includes everything the provider returned, even if not shown in chat.

## Data Structure

```markdown
# Prospect List — {ICP nickname} — {YYYY-MM-DD}

> **Generated:** {YYYY-MM-DD}
> **ICP:** {nickname} ({short summary})
> **Provider:** {Apollo / Hunter / Lusha / ZoomInfo / Vibe / web-fallback}
> **Count:** {N} prospects returned (of {M} matches)
> **Min Fit Score:** {N}/100

## Top Prospects

| # | Name | Title | Company | Geo | Email | Phone | LinkedIn | Fit |
|---|------|-------|---------|-----|-------|-------|----------|-----|
| 1 | {name} | {title} | {company} | {city} | {email or "—"} | {phone or "—"} | {url or "—"} | {N}/100 |
| 2 | ... |

## Fit Score Breakdown (Sample — Top 3)

### {Name 1} — {fit score}
- Title match: ✓ (Founder vs "Founder / CEO")
- Industry match: ✓ (SaaS)
- Company size: ✓ (12 employees, target 1-50)
- Geography: ✓ (US)
- Tech stack signal: HubSpot detected
- Email: verified

### {Name 2} ...

## Signals & Notes

- {Company X} just raised a Series A — strong timing
- {Company Y} hired a new VP of Sales — buying trigger met
- {Person Z} has been posting about [pain point] on LinkedIn

## Next Steps

- Enrich any name not fully populated: `/prospect-enrich [name] --company [company]`
- Push qualified prospects into pipeline: `/prospect-handoff --list "Prospect List — {nickname} — {date}"`
- Generate outreach: `/comm-email-drafter` or `/outreach-seller-letter`
```

## Output Format (Chat)

```
🎯 PROSPECT FIND — {ICP nickname}
Provider: {provider} • Returned {N} of {M} matches • Min fit: {N}/100

TOP 10
1. {Name} — {Title} @ {Company} — {Geo} — Fit: {N} — {email or "no email"}
2. ...

SIGNALS
• {company} {trigger}
• {company} {trigger}

Full list saved: pipeline/prospects/Prospect List — {nickname} — {date}

NEXT
- Enrich missing emails: /prospect-enrich
- Push to pipeline: /prospect-handoff --list "{list note name}"
```

## Example Usage

**User:** "Find me 25 prospects matching my agency-clients ICP"

**AI:** Loads `agency-clients` ICP. Detects Apollo MCP. Runs Apollo search with translated filters. Scores and ranks. Returns top 25 (and shows top 10 in chat). Saves full list.

**User:** "/prospect-find --icp re-sellers-utah --count 50 --geo 'Salt Lake County'"

**AI:** Loads RE seller ICP. No B2B enrichment MCP is appropriate here — uses web fallback (county records, expired listings sites). Returns 50 best matches. Lower fit scores expected.

**User:** "Find me contacts at these companies: Acme, BetaCorp, GammaCo. Need decision-makers."

**AI:** No ICP-based search — instead does targeted company-by-company. Pulls decision-maker titles from active ICP if exists, else uses defaults (Founder, CEO, COO, VP). Returns 3-5 contacts per company.

**User:** "Find prospects but require verified email"

**AI:** Same search, but drops anyone without verified email. Notes if this drops result count significantly.

## Error Handling

- **If no ICP is defined:** Stop and direct: "No ICP found. Run `/prospect-icp-define` first — searches without an ICP produce too many false positives."
- **If preferred provider MCP isn't installed:** Fall back to next-best provider. Warn user: "Hunter wasn't available. Used web fallback — email coverage will be lower. Install `hunter` plugin for better results."
- **If no provider at all and web fallback returns < 5 results:** Inform: "Web fallback found only {N} matches. Either widen the ICP or install a provider plugin (Apollo / Hunter / Lusha / ZoomInfo / Vibe)."
- **If the provider returns nothing:** Suggest one constraint to relax. Show what was filtered: "0 matches — your ICP is very tight. The narrowest filter was {field}: {value}. Try widening it."
- **If verified-email-only would drop the count below the requested count:** Warn and let user choose: "Verified-email-only would return {N} results, you asked for {M}. Continue with {N}, or include best-effort emails?"
- **If a prospect is already in `pipeline/` as a lead:** Mark them in the list with `[already in pipeline — {stage}]`. Don't duplicate.
- **If the user passes both `--icp` and one-off filters:** Filters override ICP for that run only. Don't modify the saved ICP.
- **If provider rate-limits / errors mid-search:** Save what was returned so far. Note: "Provider rate-limited after {N} results. Retry later with `/prospect-find --resume`."

## See Also

- `/prospect-icp-define` — define your ICP first (same plugin)
- `/prospect-enrich` — fill in missing fields per contact (same plugin)
- `/prospect-bulk-list` — campaign-sized list generation (same plugin)
- `/prospect-handoff` — push results into your CRM pipeline (same plugin)
- `/bizops-lead-tracker` — track these contacts as leads (from `business-operations`)
