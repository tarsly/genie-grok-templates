---
name: mkt-cro-audit
description: >-
  Conversion Rate Optimization audit for any page — homepage, landing page, pricing, feature, lead-capture, signup, or onboarding flow. Fetches the URL, scores the page against a 12-point CRO framework (clarity, headline, CTA placement, social proof, friction, trust signals, mobile, speed, copy, visuals, urgency, risk reversal), and returns a prioritized fix list with estimated lift.
---

# CRO Audit

## Overview

Audits a single page (or URL) against a 12-point CRO framework and produces a prioritized fix list with estimated conversion lift. Output is saved to Cloud Brain so audits can be re-run quarterly and compared.

If `brand-toolkit` is installed, audit uses the brand voice and offer context for sharper recommendations. If `mkt-landing-page` is also installed, audit can generate replacement copy directly.

## When This Skill Applies

- User asks to audit a page ("CRO audit my homepage", "why isn't this landing page converting?", "review my pricing page")
- User pastes a URL and asks for feedback on conversion
- User mentions: CRO, conversion rate, A/B test ideas, page audit, landing page review
- User reports a drop in conversions and wants diagnostic

## Pre-Flight — Preferences

1. Search Cloud Brain: `search_notes` for `"marketing engineering preferences"` in `brain/preferences`
2. **If found:** Load, banner, proceed
3. **If not found:** Ask in ONE message:
   - Primary website / brand URL
   - Default offer (1 sentence — what you sell, to whom)
   - Primary CTA verb (Book / Buy / Subscribe / Get / Start / Try)
   - Average page conversion rate today (if known)
   - Industry / vertical
   - Save to Cloud Brain: `write_note` → title: `marketing-engineering-preferences`, folder: `brain/preferences`
4. Banner:
   ```
   🎯 CRO Audit | Brand: {url} | Offer: {offer} | Industry: {industry}
   ```

## How It Works

### Step 1: Fetch and Parse

`WebFetch` the URL. Extract:
- HTML structure (headings, sections, CTAs, forms)
- Visible copy (above the fold, below the fold)
- Form fields and length
- Images / video alt text
- Trust signals (logos, testimonials, badges, security indicators)
- Page weight indicators (if returned by the response headers)
- Meta description, OG tags

If page is JS-heavy and WebFetch returns mostly markup, note the limitation and continue with what's visible.

### Step 2: Identify Page Type

If `--page-type` wasn't passed, infer from URL pattern and content:
- `/`, `/home` → homepage
- `/pricing`, `/plans` → pricing
- `/signup`, `/get-started` → signup
- `/{offer-slug}` with form above the fold → landing page
- `/features/{x}` → feature
- `/onboarding`, `/welcome` → onboarding

### Step 3: Score the 12 CRO Dimensions

| # | Dimension | What to check | Weight |
|---|-----------|---------------|--------|
| 1 | Clarity | Above-the-fold answers "what is this, for whom, why now?" in <5 seconds | 12 |
| 2 | Headline | Outcome-focused, specific, addresses primary objection or desire | 10 |
| 3 | Primary CTA | Single dominant CTA, action verb, visually distinct, repeated | 10 |
| 4 | Social Proof | Logos, counts, testimonials with names/photos/results | 9 |
| 5 | Friction | Form length, required fields, click count to convert | 9 |
| 6 | Trust signals | Money-back, security badges, real address, support availability | 8 |
| 7 | Mobile | Layout, tap targets, form usability, scroll behavior | 8 |
| 8 | Speed (LCP/CLS) | Hero image, third-party scripts, font loading | 8 |
| 9 | Copy specificity | Numbers, named outcomes, vs. vague claims | 7 |
| 10 | Visuals | Hero shows the product/outcome; no stock-photo abstraction | 7 |
| 11 | Urgency / scarcity | Honest urgency (cohort closes, limited slots), not fake countdowns | 6 |
| 12 | Risk reversal | Guarantee, trial, cancel-anytime, refund policy | 6 |

Score each 0-10. Multiply by weight. Sum to 1000-point total.

### Step 4: Generate Fix List

For each low-scoring dimension, write a fix recommendation:
- **What's wrong** — 1 sentence
- **What to change to** — concrete copy or layout suggestion
- **Estimated lift** — low (1-3%) / medium (5-10%) / high (10-25%)
- **Effort** — quick (<1h) / medium (1-4h) / heavy (1+ day)
- **Why** — 1 sentence pointing to research / framework / heuristic

Sort fixes by lift / effort ratio. Top 5 are "do this week"; next 5 are "do this month".

### Step 5: Save Audit

- **title:** `CRO Audit — {domain}/{page slug} — {YYYY-MM-DD}`
- **folder:** `brain/marketing/audits`
- **tags:** `["cro-audit", "{page-type}", "{domain}"]`

Render the full audit in the note. Render the top-5 fix list in chat.

## Data Structure

```markdown
# CRO Audit — {URL} — {YYYY-MM-DD}

> **Page Type:** {type}
> **Audience:** {audience}
> **Goal:** {primary conversion goal}
> **Score:** {N}/1000

## Above the Fold (Snapshot)

| Element | What's there |
|---------|--------------|
| Headline | "{actual headline}" |
| Subhead | "{actual subhead}" |
| Hero | {image / video / nothing} |
| Primary CTA | "{button text}" → {url} |
| Trust signals | {logos / testimonials / badges / none} |
| Form (if any) | {N fields, required: {list}} |

## 12-Point Scorecard

| Dimension | Score | Weight | Weighted | Notes |
|-----------|-------|--------|----------|-------|
| Clarity | {0-10} | 12 | {N} | {1-line} |
| Headline | {0-10} | 10 | {N} | {1-line} |
| Primary CTA | {0-10} | 10 | {N} | {1-line} |
| Social Proof | {0-10} | 9 | {N} | {1-line} |
| Friction | {0-10} | 9 | {N} | {1-line} |
| Trust signals | {0-10} | 8 | {N} | {1-line} |
| Mobile | {0-10} | 8 | {N} | {1-line} |
| Speed | {0-10} | 8 | {N} | {1-line} |
| Copy specificity | {0-10} | 7 | {N} | {1-line} |
| Visuals | {0-10} | 7 | {N} | {1-line} |
| Urgency | {0-10} | 6 | {N} | {1-line} |
| Risk reversal | {0-10} | 6 | {N} | {1-line} |
| **TOTAL** | | | **{N}/1000** | |

## Top 5 Fixes (Do This Week)

### Fix 1: {short title}
- **What's wrong:** {1 sentence}
- **What to change to:** {concrete suggestion or example copy}
- **Estimated lift:** {low / med / high}
- **Effort:** {quick / med / heavy}
- **Why:** {framework / research / heuristic ref}

### Fix 2 ...

## Next 5 Fixes (Do This Month)

### Fix 6 ...

## Backlog (Nice-To-Have)

- {item}
- {item}

## What's Working

- {1-3 things to NOT break in the redesign}

## Test Plan

- A/B test: {hypothesis}, control vs. variant, primary metric, traffic target, runtime estimate
```

## Output Format (Chat)

```
🎯 CRO AUDIT — {domain}/{slug}
Score: {N}/1000 • Page type: {type}

WEAK (≤5/10)
• {dimension} — {1-line issue}
• {dimension} — {1-line issue}

STRONG (≥8/10)
• {dimension}
• {dimension}

TOP 5 FIXES (THIS WEEK)
1. {fix title} — lift: {level}, effort: {level}
2. ...

A/B TEST IDEA
Hypothesis: {hypothesis}
Variant: {what to change}
Primary metric: {metric}

Full audit: brain/marketing/audits/CRO Audit — {url} — {date}
```

## Example Usage

**User:** "Audit the provider for CRO"

**AI:** Fetches the URL. Identifies it as a homepage. Scores all 12 dimensions. Detects weak headline (vague), missing social proof, primary CTA below the fold. Top 5 fixes prioritized by lift/effort. Saves audit.

**User:** "/mkt-cro-audit https://example.com --page-type landing --goal 'waitlist signup'"

**AI:** Landing page audit. Goal: waitlist. Scores trust, urgency, friction harder for waitlist-specific patterns. Generates fix list for the signup-page CRO playbook.

**User:** "Why isn't my pricing page converting? It's at /pricing"

**AI:** Resolves the URL (from brand preferences). Audits pricing-page-specific dimensions (plan comparison clarity, anchor pricing, annual/monthly toggle visibility, money-back guarantee). Returns diagnostic + fixes.

**User:** "Re-audit the homepage I audited 90 days ago — show me what changed"

**AI:** Reads the prior audit from `brain/marketing/audits/`. Runs fresh audit. Diffs scores per dimension. Notes which prior fixes were applied. Shows lift gained / regressions.

## Error Handling

- **If WebFetch can't reach the URL:** Report and suggest checking the URL is public. Offer to audit a paste of the HTML.
- **If page is JS-heavy and WebFetch returns mostly empty markup:** Note the limitation. Audit what's visible. Recommend running through a headless browser (if `playwright` MCP is available) or pasting rendered content.
- **If URL is behind auth (signup wall, checkout flow):** Ask user to paste the rendered page or describe what's behind the wall.
- **If page is a paid landing page (Unbounce / Leadpages / similar):** Still works — note in audit which platform appears to be in use.
- **If the brand preferences aren't set:** Ask once. The audit needs the offer + audience to score copy specificity.
- **If user has `mkt-landing-page` installed:** End with: "Run `/mkt-landing-page --from-audit {audit note}` to generate replacement copy for the weak sections."
- **If user has `brand-toolkit` brand kit set up:** Use its voice rules when scoring copy ("does the copy match brand voice?"). Reference brand colors / typography for visual scoring.
- **If page has unusual format (one-pager scroll, interactive demo, video-first):** Adapt scoring — note that traditional CRO heuristics apply less. Score based on intent the page is built for.
- **If score is below 400/1000:** Recommend a full redesign rather than incremental fixes. Note clearly: "Below 400 means most fixes are rearranging deck chairs. Consider rewriting the page from scratch."

## See Also

- `/mkt-landing-page` — generate replacement copy for the weak sections (same plugin)
- `/mkt-paywall-popup` — add the popup the audit flagged as missing (same plugin)
- `/mkt-seo-audit` — traditional SEO counterpart (same plugin)
- `/brandtoolkit-brand-audit` — broader brand-level audit (from `brand-toolkit`)
