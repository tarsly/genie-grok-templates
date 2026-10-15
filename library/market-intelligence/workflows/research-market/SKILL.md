---
name: research-market
description: >-
  Market research — TAM/SAM/SOM, industry analysis, market trends, growth rates, competitive landscape, key players, market validation, niche research, or wants TAM/SAM/SOM estimates, industry trends, or a market validation summary for a niche or product idea.
---

# Market Research

## Overview

Conducts comprehensive market research for any industry, niche, product, or service. Uses live web search to pull current data, then synthesizes findings into a structured report covering market size (TAM/SAM/SOM), trends, growth rates, key players, opportunities, and threats. Designed to give the user a clear picture of market dynamics for decision-making — specifically when the question is "how big is this?" or "is there a market for this?"

## When This Skill Applies

- User asks about the size of any market or industry
- User wants TAM/SAM/SOM analysis
- User asks "is there a market for X?"
- User wants to understand trends in an industry
- User asks about competitors or key players in a space
- User wants a market overview before launching a product or service
- User asks about growth rates, projections, or forecasts
- User wants opportunity or gap analysis for a niche
- User mentions market validation or feasibility research

## How It Works

### Step 1: Define the Research Scope

Extract from the user's request:

| Parameter | Description | Example |
|-----------|-------------|---------|
| Industry/Niche | What market to research | "B2B SaaS for construction" |
| Geography | Market boundary — use saved preference as default | US, Global, specific region |
| Timeframe | Analysis period | Current + 5-year forecast |
| Depth | How deep to go | Quick overview vs. deep dive |
| Specific Questions | Anything particular they want answered | "Is there room for a new entrant?" |

### Step 2: Conduct Web Research

Run multiple targeted web searches to gather current data:

**Search queries to run (adapt to the specific market):**

1. `"{industry} market size 2025 2026"` — Current market valuation
2. `"{industry} market growth rate CAGR forecast"` — Growth projections
3. `"{industry} industry trends 2026"` — Current and emerging trends
4. `"{industry} key players companies market share"` — Competitive landscape
5. `"{industry} challenges problems pain points"` — Market gaps and opportunities
6. `"{industry} emerging technology disruption"` — Innovation and disruption vectors
7. `"{industry} consumer behavior demographics"` — Buyer/user analysis
8. `"{industry} regulations policy changes 2026"` — Regulatory environment

Run at least 4-6 searches. More for deep dives.

### Step 3: Synthesize Market Size (TAM/SAM/SOM)

Calculate or estimate using data found:

| Metric | Definition | How to Estimate |
|--------|-----------|-----------------|
| **TAM** (Total Addressable Market) | Total revenue opportunity if 100% market share | Industry-wide revenue or volume |
| **SAM** (Serviceable Addressable Market) | Portion of TAM targetable by the user's specific product/model | Filter by geography, segment, channel |
| **SOM** (Serviceable Obtainable Market) | Realistic share capturable in 1-3 years | SAM filtered by competition, resources, reach |

If exact numbers are not available, use triangulation:
- Top-down: Total industry size, filter down
- Bottom-up: Number of potential customers x average revenue per customer
- Comparable: Similar markets or adjacent industries as proxies

Always note confidence level: **High** (multiple sources agree), **Medium** (extrapolated from partial data), **Low** (estimated from limited info).

### Step 4: Analyze Competitive Landscape

For key players found:

| Competitor | What They Do | Est. Revenue/Size | Strengths | Weaknesses |
|-----------|-------------|-------------------|-----------|------------|
| {name} | {description} | {size} | {strengths} | {gaps} |

Identify:
- Market leaders and their market share
- Emerging challengers
- Recent entrants or exits
- Consolidation trends (M&A activity)
- White space where no one is serving well

### Step 5: Identify Trends and Opportunities

Categorize trends:

| Trend Type | Examples |
|-----------|---------|
| Technology | AI adoption, platform shifts, automation |
| Consumer | Behavior changes, preference shifts, demographics |
| Regulatory | New laws, compliance requirements, deregulation |
| Economic | Interest rates, inflation impact, funding environment |
| Competitive | New entrants, consolidation, pricing pressure |

For each trend, assess: **Impact** (high/medium/low) and **Timeline** (happening now / 1-2 years / 3-5 years).

### Step 6: SWOT for Market Entry

If the user is evaluating whether to enter or expand in a market:

| | Positive | Negative |
|---|---------|----------|
| **Internal** | Strengths: What the user brings | Weaknesses: What they lack |
| **External** | Opportunities: Market gaps | Threats: Risks and barriers |

### Step 7: Compile and Save Report

Save the research report to cloud brain using `mcp__cloud-brain__write_note` with:
- `--title` "{topic-slug}-market-research"
- `--folder` "brain/research"
- `--tags` "market-research, research, {industry keywords}"

Check for an existing note first via `mcp__cloud-brain__search_notes`. If one exists, ask: "A market research report on '{topic}' already exists. Refresh it with current data, or create a new version?"

## Output Format

```markdown
# Market Research: {Industry/Niche}

> **Date:** {YYYY-MM-DD}
> **Scope:** {geography, segment}
> **Depth:** {quick overview / deep dive}
> **Confidence Level:** {high / medium / low}

---

## Executive Summary

{3-5 sentence overview of the market, its size, trajectory, and the key takeaway}

---

## Market Size

| Metric | Value | Confidence |
|--------|-------|------------|
| TAM | ${amount} | {high/medium/low} |
| SAM | ${amount} | {high/medium/low} |
| SOM | ${amount} | {high/medium/low} |
| CAGR | {X%} ({period}) | {high/medium/low} |

**Methodology:** {how the numbers were derived — top-down, bottom-up, comparable}

**Sources:** {list key sources and data points used}

---

## Key Trends

### 1. {Trend Name}
- **Impact:** {High/Medium/Low}
- **Timeline:** {Now / 1-2 years / 3-5 years}
- **Details:** {explanation}

---

## Competitive Landscape

| Player | Description | Est. Size | Market Position |
|--------|------------|-----------|-----------------|
| {name} | {what they do} | {revenue/users} | {leader/challenger/niche} |

### White Space / Gaps
- {gap 1}
- {gap 2}

---

## Opportunities

1. **{Opportunity}** — {description, why it matters, potential size}
2. **{Opportunity}** — {description}

## Threats & Risks

1. **{Risk}** — {description, likelihood, mitigation}
2. **{Risk}** — {description}

---

## SWOT Analysis

| | Favorable | Unfavorable |
|---|----------|-------------|
| **Internal** | {Strengths} | {Weaknesses} |
| **External** | {Opportunities} | {Threats} |

---

## Recommendations

1. {Actionable recommendation based on findings}
2. {Actionable recommendation}
3. {Actionable recommendation}

---

## Sources

- {Source 1 — URL or publication}
- {Source 2}

---

*Market research conducted {date} by the provider AI. Market data should be refreshed quarterly.*
```

## Example Usage

**User:** "How big is the AI SaaS market for small businesses?"

**AI:** Runs 6-8 web searches, finds market size data, builds TAM/SAM/SOM estimates with confidence levels, identifies key players and white space. Delivers quick overview or standard depth based on saved preference.

**User:** "Is there a market for a billing automation tool targeting solo contractors?"

**AI:** Researches the niche with bottom-up sizing (number of US solo contractors × billing tool penetration rate), identifies direct competitors, evaluates white space and barriers to entry. Delivers a market entry assessment with GO/CAUTION/PASS signal.

**User:** "Deep dive on the online education market for professional certifications"

**AI:** Deep dive — multiple searches, detailed competitive analysis, pricing comparison, student demographics, conversion rates, and specific opportunities for a new entrant.

## Error Handling

- **If WebSearch is unavailable:** Inform the user and offer to proceed from training data with a lower confidence flag.
- **If web search returns limited data for a niche market:** Use triangulation methods and note the confidence level. "Limited direct data available — estimates derived from {method} with {low/medium} confidence. Cross-reference before major decisions."
- **If the user does not specify a market:** Ask: "What market or industry do you want me to research? The more specific, the better — 'creative finance for vehicles in the US' is better than 'finance'."
- **If TAM/SAM/SOM data is conflicting:** Show calculation methodology, present ranges rather than single numbers, and flag: "These are estimates. For precise market sizing, consider commissioning a formal market study."
- **If the user asks for a geography not well-covered by English-language search:** Note: "Data for {geography} may be limited in English-language sources. Results are based on what I could find. Consider local language research or regional industry reports for deeper coverage."
