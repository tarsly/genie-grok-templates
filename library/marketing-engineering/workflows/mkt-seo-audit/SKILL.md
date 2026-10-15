---
name: mkt-seo-audit
description: >-
  Traditional SEO audit for a URL or full site — technical SEO (crawlability, indexation, robots.txt, sitemap, canonical, hreflang, schema), on-page SEO (title tags, meta descriptions, H1, internal linking, content depth, keyword targeting), and content SEO (search intent match, content gaps, competing pages). Returns a prioritized fix list with estimated traffic lift.
---

# SEO Audit

## Overview

Classic SEO audit covering technical, on-page, and content factors. Returns a prioritized fix list with effort and estimated lift. Pairs with `mkt-ai-seo` (the AI-first companion) and `mkt-cro-audit` (the conversion companion). Most modern strategies need all three.

## When This Skill Applies

- User asks for an SEO audit ("audit my SEO", "why isn't my site ranking?")
- User has a target keyword and wants the page optimized for it
- User wants to know how their site compares to competitors organically
- User mentions: SEO audit, technical SEO, on-page SEO, content audit, page speed, indexation
- User just launched a site and wants the baseline audit before pushing traffic

## Pre-Flight — Preferences

Loads `marketing engineering preferences`. Asks one additional question:
- Do you have Google Search Console access? (yes — paste a CSV / no — work without)
- Any target keywords already known? (list / none yet)

Banner:
```
🎯 SEO Audit | Scope: {page/site} | Brand: {url} | GSC: {available/no}
```

## How It Works

### Step 1: Determine Scope

- `--scope page` → single URL audit (default if URL ends in a slug)
- `--scope site` → full site audit (crawl from homepage, depth ~3, max 200 pages)

For site audits, identify a representative sample if full crawl isn't possible via WebFetch.

### Step 2: Technical SEO Checks

| Check | What to verify |
|-------|---------------|
| Robots.txt | Reachable at `/robots.txt`, doesn't block important paths |
| Sitemap | Reachable at `/sitemap.xml`, listed in robots.txt, contains key URLs |
| Canonical | Every page has `<link rel="canonical">`, pointed correctly |
| HTTPS | All URLs https, no mixed content |
| HTTP status | Pages return 200, no chains of redirects, no 404s in nav |
| Indexability | No `noindex`, no `disallow` on indexable pages |
| Mobile-friendly | Viewport meta, responsive layout |
| Page speed | LCP < 2.5s, CLS < 0.1 (approximate from page weight + render-blocking) |
| Schema | At least Organization + WebSite + appropriate type per page |
| hreflang | If multi-language, correct hreflang annotations |
| Pagination | rel=next/prev or correct canonical handling |
| Structured nav | Main nav with all top-level sections; breadcrumbs |
| URL structure | Lowercase, hyphens, shallow depth, descriptive |
| 4xx / 5xx | No widespread errors |
| JS-rendering | Critical content not blocked behind JS-only render |

### Step 3: On-Page SEO Checks

For each audited page:
- **Title tag:** present, ≤60 chars, includes primary keyword near front, brand at end
- **Meta description:** present, 130-160 chars, includes keyword, has clear CTA
- **H1:** single H1, contains primary keyword
- **Heading hierarchy:** H2/H3 used logically, no skips
- **Body content:** ≥300 words for indexed pages (≥800 for high-intent pages)
- **Keyword density:** primary keyword in first 100 words, ~1% density (avoid stuffing)
- **Internal links:** ≥3 internal links, anchor text descriptive
- **External links:** outbound links to authoritative sources where claims are made
- **Images:** all have alt text, lazy-load, properly sized
- **Open Graph / Twitter Card:** present
- **JSON-LD:** at least Article / Product / FAQ as appropriate
- **Last updated date:** visible

### Step 4: Content SEO Checks

For target keyword(s):
- **Search intent match:** does the page match what searchers actually want? (informational / transactional / comparison / navigational)
- **SERP analysis:** what's currently ranking #1-#5? what content patterns do they share?
- **Content depth:** is the page substantially better than #5 result?
- **Topical coverage:** are related entities and questions covered? (use People Also Ask + related searches as proxy)
- **E-E-A-T signals:** experience, expertise, authoritativeness, trust — bylines, author bios, sources
- **Freshness:** when was the page last updated? Is updating warranted?
- **Cannibalization:** do you have multiple pages competing for the same keyword?

### Step 5: Competitive Analysis (Optional)

If `--competitors` is passed:
- Fetch each competitor URL ranking for the same keyword
- Compare title, meta, H1, word count, schema, structure
- Identify what they do better

### Step 6: Generate Fix List

For each issue, write:
- **What's wrong** — 1 sentence
- **What to change to** — concrete fix
- **Category** — technical / on-page / content
- **Estimated lift** — none (cleanup) / small (1-5%) / medium / large
- **Effort** — quick / med / heavy

Sort by `lift / effort`. Top 5 = "do this week". Next 10 = "do this month". Rest = backlog.

### Step 7: Save

- **title:** `SEO Audit — {url or domain} — {YYYY-MM-DD}`
- **folder:** `brain/marketing/audits`
- **tags:** `["seo-audit", "{scope}", "{domain}"]`

## Data Structure

```markdown
# SEO Audit — {URL} — {YYYY-MM-DD}

> **Scope:** {page / site}
> **Target keyword(s):** {list}
> **Pages audited:** {N}
> **Issues found:** {N total — {tech} technical, {onpage} on-page, {content} content}

## Executive Summary

- **Technical health:** {N}/10
- **On-page health:** {N}/10
- **Content health:** {N}/10
- **Top win:** {1 sentence}
- **Top blocker:** {1 sentence}

## Technical SEO

### Robots / Sitemap / Indexation
- robots.txt: {status}
- sitemap.xml: {status}
- Indexable pages: {N} of {M}
- Issues: {list}

### Performance
- Estimated LCP: {Ns}
- Estimated CLS: {N}
- Mobile-friendly: {yes/issues}

### Schema
- Found: {list}
- Missing recommended: {list}

### Other Technical Issues
- {issue} — fix: {fix}
- ...

## On-Page SEO

### Page-by-Page Findings
| URL | Title | H1 | Word ct | Issues |
|-----|-------|-----|---------|--------|
| {url} | {title len, score} | {present, kw match} | {N} | {short} |
| ... |

### Common Patterns
- {N} pages missing meta descriptions
- {N} pages with duplicate titles
- {N} pages with no internal inbound links

## Content SEO

### Target Keyword: {keyword}
- Current ranking: {position or "not ranking in top 100"}
- Top result analysis: {who, what they do, why they win}
- Your page vs theirs: {gap analysis}

### Content Gaps
- Topics related to {keyword} that you don't cover: {list}
- Long-tail variants worth building: {list}

### Cannibalization
- {N} pages competing for {keyword} → consolidate or differentiate

## Top 5 Fixes (Do This Week)

### Fix 1: {short title}
- **Category:** {tech / onpage / content}
- **Issue:** {1 sentence}
- **Fix:** {concrete action}
- **Lift:** {small / med / large}
- **Effort:** {quick / med / heavy}

### Fix 2 ...

## Next 10 (Do This Month)

...

## Backlog

...

## Notes

- Re-audit in 90 days to track delta.
- Cross-reference with `mkt-ai-seo` audit for AI search visibility.
- Cross-reference with `mkt-cro-audit` for conversion (SEO without CRO = wasted traffic).
```

## Output Format (Chat)

```
🎯 SEO AUDIT — {url}
Technical: {N}/10 • On-page: {N}/10 • Content: {N}/10

TOP BLOCKER
{1-2 sentences — most impactful single issue}

TOP 5 FIXES (THIS WEEK)
1. {fix} — lift: {level}, effort: {level}
2. ...

QUICK WINS (< 1 hour)
- {item}
- {item}

CONTENT GAPS
- {gap}
- {gap}

Full audit: brain/marketing/audits/SEO Audit — {url} — {date}
```

## Example Usage

**User:** "Audit the provider for SEO"

**AI:** Site-scope audit. Crawls representative sample. Reports technical / on-page / content scores. Top fixes prioritized. Saved.

**User:** "/mkt-seo-audit <your-provider-settings-url> --target-keyword 'Claude Code plugins for business'"

**AI:** Page-scope audit against the target keyword. SERP analysis. Returns gap vs top results.

**User:** "Audit my SEO and compare to my top 3 competitors"

**AI:** Asks for competitor URLs. Audits each at the homepage level. Generates side-by-side comparison.

**User:** "My organic traffic dropped 30% last month — diagnose"

**AI:** Audit + ask user to paste GSC data if they have access. Cross-reference fix list with recent changes (git log of site repo if accessible). Suggest top 3 hypotheses (algorithm update, indexation issue, content quality issue).

## Error Handling

- **If site is JS-only and WebFetch returns empty:** Recommend running through playwright MCP if installed (detected via `mcp__playwright__*` tools), or pasting rendered HTML. Limited audit possible from server response alone.
- **If site is behind auth:** Audit publicly-accessible portion only.
- **If user wants ranking data and GSC is unavailable:** Use third-party SERP checks if accessible; otherwise be honest about limits.
- **If competitor URLs given but pages are gated/paywalled:** Audit what's visible, note limitations.
- **If site has zero schema:** That's a major opportunity — call out as top win.
- **If site has aggressive `noindex` / `disallow` settings:** Check whether intentional (staging) or accident (live site blocked).
- **If user asks for tactics the audit didn't surface (link building, etc.):** Note: "This audit covers technical, on-page, content. Off-page (links, citations, mentions) is a separate workstream — recommend running a backlink audit via Ahrefs/Semrush exports."
- **If `mkt-ai-seo` is installed** (always true in this plugin — same package): Append to the final summary the line `See also: /mkt-ai-seo --audit for the AI-search counterpart audit. Modern strategies need both.`

## See Also

- `/mkt-ai-seo` — AI-search counterpart — modern strategy needs both (same plugin)
- `/mkt-cro-audit` — conversion audit on the same pages (same plugin)
- `/mkt-programmatic-seo` — fix gaps by generating high-quality pages at scale (same plugin)
