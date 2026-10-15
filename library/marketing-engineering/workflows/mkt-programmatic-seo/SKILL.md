---
name: mkt-programmatic-seo
description: >-
  Programmatic SEO — generate hundreds or thousands of high-quality location, keyword, comparison, or directory pages from a single template. Produces the template, the data source spec (CSV/JSON), the URL pattern, the page schema, and sample output for QA. Works for real estate ('homes for sale in {city}'), local services ('{service} in {city}'), software directories ('best {category} for {use case}'), and SaaS comparison pages ('{tool-a} vs {tool-b}').
---

# Programmatic SEO

## Overview

Programmatic SEO produces N pages from one template by varying one or two data axes (location × keyword, tool × tool, category × use case). Done badly, it's thin doorway pages Google demotes. Done well, it owns long-tail search at scale.

This skill produces the four artifacts you need to do it well:
1. **The template** — page structure with merge fields
2. **The data source spec** — what fields the CSV/JSON needs
3. **The URL pattern** — clean, hierarchical, indexable
4. **The QA checklist** — verify each page has unique value, not just regex-replaced fluff

Output is hand-off-ready to a developer or no-code stack.

## When This Skill Applies

- User mentions: programmatic SEO, pSEO, location pages, directory site, comparison pages, scaled SEO
- User has a target market (e.g., "every metro in the US") and wants pages for each
- User is building a directory or comparison site
- Real estate: "homes for sale in {city}", "{neighborhood} market trends"
- Local service: "{service} near me", "{service} in {city}"
- SaaS: "{tool} vs {tool}", "best {category} for {use case}"

## Pre-Flight — Preferences

Reads marketing-engineering preferences. Asks one additional question on first run:
- What's your CMS / static site generator? (Next.js / Webflow / WordPress / Astro / Hugo / custom)
- Where will the pages live? (subdomain / subdirectory like `/locations/`)

Banner:
```
🎯 Programmatic SEO | CMS: {cms} | URL pattern: {pattern}
```

## How It Works

### Step 1: Pick the Variant

Each variant has different template requirements:

**Location pages** — `{keyword} in {city}`
- Data axes: keyword (1-N), city (50-5000)
- Examples: "homes for sale in Salt Lake City", "best dentists in Provo"
- Required dynamic data: local stats, local examples, local providers/listings, local images
- Common mistake: just templated boilerplate with city name swapped → Google penalty

**Comparison pages** — `{tool-a} vs {tool-b}`
- Data axes: pairs of tools
- Examples: "Notion vs Obsidian", "Hunter vs Apollo"
- Required dynamic data: feature comparison, pricing, screenshots, use-case fit, verdicts
- Common mistake: identical "vs" pages with shallow comparison — needs actual differences

**Directory pages** — `best {category} for {use case}`
- Data axes: category, use case
- Examples: "best CRMs for real estate agents", "best AI tools for solopreneurs"
- Required dynamic data: 10-20 listings, criteria explanations, expert picks, decision tree
- Common mistake: thin lists scraped from competitors — needs editorial layer

**Use-case pages** — `{tool} for {use case}`
- Data axes: tool, use case
- Examples: "Claude Code for real estate investors", "Reclaim.ai for solopreneurs"
- Required dynamic data: specific workflow, tutorial, screenshot, results, edge cases
- Common mistake: AI-generated walls of text. Needs original screenshots + numbers.

### Step 2: Design the URL Pattern

Recommended patterns:

- Location: `/locations/{state-slug}/{city-slug}/{keyword-slug}`
- Comparison: `/compare/{tool-a}-vs-{tool-b}`
- Directory: `/best/{category}/{use-case}`
- Use case: `/{tool}/for/{use-case}`

Rules:
- All lowercase, hyphens (not underscores)
- Hierarchical so users can crawl up
- Avoid query parameters
- Canonical tag on every page (self-canonical)

### Step 3: Generate the Template

The skill outputs a Markdown template with merge fields, plus a JSON or CSV schema showing what fields the data source needs.

Template structure (per variant — example for Location):

```markdown
# {keyword} in {city}, {state}

> Last updated: {{updated_at}}

## Overview

In {city}, {keyword} {trend_sentence — generated from data}. The {city} market has {N} active listings, with a median price of ${median_price} and average days on market of {dom}.

[Local hero image: {{hero_image_url}}]

## Why {city}?

{{city_why_paragraph — pulled from local data: schools, commute, climate, growth}}

## Current {keyword} Listings in {city}

{{listings_table}}

## {city} Market Trends

[Chart: median price last 12 months in {city}]

- Median price: ${median_price} ({delta_pct} vs last year)
- Average DOM: {dom} days
- Inventory: {N} active

## Compare {city} to Nearby Markets

| Market | Median | DOM | Inventory |
|--------|--------|-----|-----------|
| {city} | ${median_price} | {dom} | {N} |
| {nearby_1} | ${nearby_1_median} | {nearby_1_dom} | {nearby_1_inv} |
| {nearby_2} | ${nearby_2_median} | {nearby_2_dom} | {nearby_2_inv} |

## Local Experts

{{local_agents — 3-5, real}}

## FAQ

**Q: How much does a {keyword_singular} cost in {city}?**
A: {answer pulled from data}

**Q: Is {city} a good place to {keyword_verb}?**
A: {answer pulled from data + 2 sentences of analysis}

## Schema Markup

{JSON-LD: LocalBusiness + Article + FAQPage}
```

### Step 4: Specify the Data Source

Generate the CSV/JSON schema. Example for Location variant:

```json
{
  "cities": [
    {
      "city_slug": "salt-lake-city",
      "city_name": "Salt Lake City",
      "state_slug": "ut",
      "state_name": "Utah",
      "metro_pop": 1257936,
      "median_price": 525000,
      "median_price_delta_pct": -2.3,
      "dom": 38,
      "active_listings": 1248,
      "hero_image_url": "https://...",
      "city_why_paragraph": "...",
      "trend_sentence": "...",
      "nearby_markets": [
        { "city": "Provo", "median": 480000, "dom": 41, "inv": 580 }
      ],
      "local_agents": [
        { "name": "...", "phone": "...", "url": "..." }
      ],
      "faq_cost_answer": "...",
      "faq_good_place_answer": "...",
      "updated_at": "2026-06-20"
    }
  ]
}
```

Identify data sources:
- Public: census.gov, BLS, OpenStreetMap, county records
- Paid APIs: Zillow GetSearchResults (real estate), Yelp Fusion (local biz), Crunchbase (SaaS)
- Original: data the user collects (surveys, internal usage)
- Manual: editorial fills (city_why_paragraph, trend_sentence) — write 3 templates that vary based on data flags (positive trend / flat / declining)

### Step 5: QA Checklist

Programmatic SEO fails when pages are too templated. Output a QA checklist:

1. **Does each page have at least one unique data point not on any other page?** (e.g., local-only stat, local-only photo, local quote)
2. **Does the lead paragraph vary?** (Multiple paragraph templates triggered by data conditions, not the same paragraph with name swapped)
3. **Is there a unique image per page?** (Stock-photo regression is a top failure mode)
4. **Does the page answer the searcher's likely follow-up?** (If they came for "homes for sale in {city}", they probably also want to know about schools, taxes, commute)
5. **Internal links?** (Each page links to 5-10 related pages — neighboring cities, related categories)
6. **Schema?** (JSON-LD present and valid)
7. **Mobile?** (Tables collapse, images responsive)
8. **Speed?** (LCP < 2.5s; lazy-load below-fold)
9. **Indexation?** (Listed in sitemap, not in robots.txt block, canonical tag)

### Step 6: Save

- **title:** `pSEO — {pattern} — {YYYY-MM-DD}`
- **folder:** `brain/marketing/seo`
- **tags:** `["programmatic-seo", "{variant}", "{pattern-slug}"]`

Include in the note:
- Template
- Data schema
- URL pattern
- QA checklist
- Suggested first 10 pages (real values plugged in) for review

## Data Structure

See sections above — the saved note contains the full template + schema + sample pages.

## Output Format (Chat)

```
🎯 PROGRAMMATIC SEO — {pattern}
Variant: {variant} • URL: {url-pattern}

GENERATED
✓ Template ({N} sections)
✓ Data schema ({M} required fields, {K} optional)
✓ URL pattern
✓ QA checklist ({L} items)
✓ Sample pages (first 3 with real data)

DATA SOURCES NEEDED
• {source 1}
• {source 2}

FIRST 10 PAGES SUGGESTED
1. /{slug-1} — {label}
2. /{slug-2} — {label}
...

NEXT
- Plug data into template
- Build first 10 pages manually
- Run /mkt-cro-audit on the first 3 once live
- Submit to sitemap

Full output: brain/marketing/seo/pSEO — {pattern} — {date}
```

## Example Usage

**User:** "Build programmatic SEO for 'homes for sale in {city}' across the top 100 US metros"

**AI:** Variant: location. Template generated with merge fields. Data schema specifies 22 fields per city. Lists data sources (Zillow API, BLS). Suggests first 10 cities by population. Saves.

**User:** "/mkt-programmatic-seo --variant comparison --pattern '{tool-a} vs {tool-b}' --seed-list 'top-25-AI-tools'"

**AI:** Generates comparison-page template. Schema lists per-pair data (features, pricing, screenshots). Outputs 24 pages × 24 pages = potential 576 comparison pages; recommends starting with 24 (one each for top-of-funnel "tool vs everyone").

**User:** "I want directory pages for 'best Claude Code plugins for {use case}' — like 'best for real estate', 'best for solopreneurs'"

**AI:** Variant: directory. Template + use-case schema. Pulls existing Genie plugins as listings. Generates 8-12 use-case pages.

**User:** "Programmatic SEO for 'best {service} in {city}' across Utah counties"

**AI:** Variant: directory + location hybrid. Lists 29 Utah counties × M services. Recommends starting with top 5 services (most search volume) × all counties = 145 pages. Generates template + Yelp Fusion data spec.

## Error Handling

- **If data sources don't exist for the proposed pattern:** Push back honestly — "There's no good data source for {field}. Either find one, generate it manually (limits scale), or pick a different pattern."
- **If user wants to generate 10,000+ pages with no original data:** Refuse politely. Google penalizes thin/duplicate pages. Recommend starting with 50-100, validating quality + indexation, then scaling.
- **If user wants to use only AI-generated content (no data, no images):** Strongly warn — pure-AI thin pages are the top cause of pSEO penalties. Require at least one unique-per-page data point.
- **If user already has pSEO live and is here for an audit:** Reframe as: "Want me to audit what you've built? Different skill flow." Then run an audit of 5 random sample pages and report findings.
- **If user's CMS doesn't support dynamic routing well (e.g., Wix):** Recommend migrating to Next.js / Astro / Webflow CMS Collections before building.
- **If the target keyword has low search volume:** Note: "{keyword} has ~{N} monthly searches. pSEO is best when long-tail volume sums to meaningful traffic. Re-check the seed."
- **If pages will compete with paid programmatic SEO sites (Yelp, Zillow):** Be honest about ranking difficulty. Recommend going more specific (one neighborhood, not one city) or building a different angle (expert curation, not scraped data).
- **If `mkt-ai-seo` is also installed** (always true in this plugin — same package): Append to the output the line `See also: /mkt-ai-seo — every pSEO page should include the AI-SEO citation patterns (lead claim, FAQ schema, named entities).`

## See Also

- `/mkt-ai-seo` — AI-SEO patterns to apply to each generated page (same plugin)
- `/mkt-seo-audit` — audit the first 5 generated pages once live (same plugin)
- `/realestate-cma-builder` — generate CMA data to feed location pages (from `realtor-toolkit`)
