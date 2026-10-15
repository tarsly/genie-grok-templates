---
name: prospect-enrich
description: >-
  Take a name (and optionally a company) and return verified contact data — work email, mobile/direct phone, LinkedIn URL, current title, prior roles, and any available buying signals. Provider-agnostic: tries the best installed enrichment MCP for the data you need, falls back to web. Designed for one-off lookups before outreach, not bulk enrichment.
---

# Prospect Enrich

## Overview

Single-contact enrichment. Where `prospect-find` returns many partial matches, `prospect-enrich` takes ONE known person and fills in everything you can find. Built for the "I have a name, give me the email" moment before outreach.

Provider routing:
- Best email-finding → Hunter (or Apollo as fallback)
- Best mobile phone → Lusha (or ZoomInfo)
- Best buying signals → ZoomInfo / Apollo
- Web fallback always available

## When This Skill Applies

- User has a name (and maybe company) and needs contact info ("get me {name}'s email", "enrich Sarah Chen at Acme")
- User pasted a LinkedIn profile and wants to message but doesn't have email
- User mentions a person and asks to "look them up" or "find their email"
- User says: "enrich", "find email", "find phone", "verify email", "look up contact"

## Pre-Flight — Preferences

Reads from `prospecting-preferences` (set up by `prospect-find` or this skill on first run):
- Available providers
- Verified-email policy (require / best-effort)
- Auto-save enriched contacts to `pipeline/` or just return inline

If preferences not set, ask the same questions as `prospect-find` and save them once.

Banner:
```
🎯 Enrich | Providers: {detected} | Verify: {on/off} | Auto-save: {yes/no}
```

## How It Works

### Step 1: Resolve Identity

Minimum required: `name`. Strongly recommended: at least one of `company`, `domain`, `linkedin`.

If only a name is provided, ask once: "What company is {name} at, or do you have their LinkedIn URL? Without one of those, I'll get false positives on common names."

Skip the ask if name is rare enough (e.g., "Jane Doe") or context implies a known person from a recent thread.

### Step 2: Check Cloud Brain First

`search_notes` for the name. If a recent enrichment exists (< 30 days old):
- Show what's already known
- Ask: "I have enrichment data from {date}. Refresh, or use cached?"

This saves API calls and respects provider quotas.

### Step 3: Provider Cascade

Run providers in priority order until all needed fields are filled, OR all providers exhausted:

**For work email:**
1. Hunter (`email-finder` by name + domain)
2. Apollo (`people/match`)
3. Vibe Prospecting (match flow)
4. Web fallback (`site:{domain} "{name}"`, parse mailto links; common email patterns: firstname.lastname@domain, first@domain, flast@domain)

**For verified email:**
1. Hunter (`email-verifier`)
2. Apollo (verified flag in person object)
3. Skip verification gracefully if no verifier MCP — note as "best-effort"

**For phone:**
1. Lusha (preferred for mobile)
2. ZoomInfo
3. Apollo (direct dial)
4. Web fallback rarely useful for phones — note as unavailable

**For LinkedIn:**
1. Any provider that returns LinkedIn URL in person object
2. Web fallback: `site:linkedin.com/in/ "{name}" "{company}"`

**For title / current role:**
1. Any provider
2. Web fallback: LinkedIn snippet, company team page

**For buying signals:**
1. Apollo / ZoomInfo (built-in intent data)
2. Otherwise: skip (this skill won't synthesize signals — that's what `brief-person-30days` is for)

### Step 4: Verify Email (Optional)

If `--verify-email` or preferences require verification:
- Use email-verifier MCP (Hunter has one)
- Mark each email as `verified | catch-all | risky | invalid`
- Drop invalid; flag catch-all and risky

### Step 5: Render and Save

Render the enrichment table inline. Save if preferences say so.

If a contact note exists in `people/`, update it. Otherwise, offer to create one but don't auto-create.

If the enrichment is for a known lead in `pipeline/`, append to the lead's note as a new "Enriched contact info" section.

## Data Structure

```markdown
# Enrichment — {Full Name} — {YYYY-MM-DD}

> **Resolved at:** {YYYY-MM-DD HH:MM}
> **Provider(s) used:** {list}
> **Confidence:** {high / medium / low}

## Contact Info

| Field | Value | Source | Verified |
|-------|-------|--------|----------|
| Full Name | {name} | — | — |
| Title | {title} | {provider} | — |
| Company | {company} | {provider} | — |
| Domain | {domain} | {provider} | — |
| Work Email | {email} | {provider} | {verified/catch-all/risky/best-effort} |
| Personal Email | {if returned} | {provider} | {status} |
| Mobile Phone | {phone} | {provider} | — |
| Direct Phone | {phone} | {provider} | — |
| LinkedIn | {url} | {provider} | — |
| X / Twitter | {handle} | {provider} | — |
| GitHub | {handle} | {provider} | — |
| City | {city} | {provider} | — |
| Timezone | {tz} | {provider} | — |

## Career

- {YYYY-present} — {Title} at {Company}
- {YYYY-YYYY} — {Title} at {Company}
- ...

## Buying Signals (if returned by provider)

- {YYYY-MM-DD} — {signal source} — {signal description}

## Notes

- {YYYY-MM-DD}: Enriched. {anything notable}
```

## Output Format (Chat)

```
✓ ENRICHED — {Name} @ {Company}
Confidence: {high/medium/low}

📧 Email:    {email} ({verified/catch-all/best-effort})
📞 Mobile:   {phone or "not found"}
📞 Direct:   {phone or "not found"}
💼 LinkedIn: {url or "not found"}
🐦 X:        {handle or "not found"}
🐙 GitHub:   {handle or "not found"}
📍 Location: {city, region}
🕐 Timezone: {tz}

CAREER
{title} at {company} ({years})
Prev: {title} at {company} ({years})

{If signals available, show top 1-2}

Saved: people/{Name}  (or "Enrichment cached in Cloud Brain")
```

## Example Usage

**User:** "Get me Sarah Chen's email — she's at Acme AI"

**AI:** Checks Cloud Brain for cached enrichment. None. Detects Hunter. Runs `email-finder` with name + domain `acme.ai`. Returns `sarah@acme.ai` (verified). Renders enrichment. Offers to save.

**User:** "/prospect-enrich Marcus Lee --linkedin linkedin.com/in/marcuslee --verify-email"

**AI:** Pulls LinkedIn URL → resolves company. Runs Hunter for email, verifier for verification. Renders.

**User:** "Look up Peter Steinberger"

**AI:** Name is recognizable; doesn't ask for company. Runs web fallback (no enrichment MCP needed for a public person) → returns X handle, GitHub, current company from LinkedIn snippet.

**User:** "Find email for everyone in my Prospect List — agency-clients — 2026-06-20"

**AI:** Stops. Says: "This is a bulk enrichment — use `/prospect-find` to refresh the list with verified emails. This skill is for single contacts."

## Error Handling

- **If name is too common and no company/domain/LinkedIn given:** Ask once. If user insists, do best effort but warn: "I'm going to guess. Wrong-person risk is high. Add `--company` or `--linkedin` for confidence."
- **If no provider MCP is available and web fallback returns nothing:** Honest report: "No email found via web. Install Hunter or Apollo for better coverage." Don't fabricate.
- **If email pattern is guessed but unverified:** Mark clearly as `best-effort, not verified`. Suggest verifying via Hunter or by sending a test email.
- **If person has clearly changed companies (LinkedIn says X, provider says Y):** Show both, prefer the more recent source. Note discrepancy: "LinkedIn shows {X}, {provider} shows {Y}. Verify before outreach."
- **If user has a `bizops-lead-tracker` lead for this person:** Append enrichment to the lead note. Don't overwrite — append.
- **If the person is in a privacy-conscious region (GDPR EU, Canada) and only personal email is available:** Note the legal context: "Found personal email but cold outreach to EU residents requires opt-in basis under GDPR. Use sparingly."
- **If the provider returns multiple matches (same name, different companies):** Show all and ask which to enrich. Never pick silently.
- **If the user asks for an email that's an obvious private/personal address (e.g., @gmail.com for someone at a real company):** Surface both — many founders prefer Gmail. Note which is which.

## See Also

- `/prospect-find` — for bulk discovery; this skill is for one-off enrichment (same plugin)
- `/brief-person-30days` — current public activity on this person (from `person-brief`)
- `/comm-email-drafter` — draft outreach now that you have the email (from `communications`)
