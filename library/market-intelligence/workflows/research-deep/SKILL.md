---
name: research-deep
description: >-
  Deep research — comprehensive, web-sourced reports on any topic. Industry analysis, trend analysis, statistics, market research, intelligence reports, or wants a comprehensive, web-sourced report on a company, person, market, or strategic question.
---

# Deep Research — Comprehensive Topic Research & Analysis

## Overview

Deep Research performs a thorough investigation of any topic using web search to find current data, statistics, trends, competitors, market sizing, expert opinions, and key insights. It compiles everything into a well-organized, citation-rich markdown research document saved to the cloud brain (folder: research). This is not surface-level Googling — it is structured, multi-angle research that produces an actionable intelligence document.

## When This Skill Applies

- User says "research [topic]" or "deep dive on [topic]"
- User says "look into [something]" or "find out about [something]"
- User says "what's the market for [X]?"
- User says "give me a breakdown of [industry/topic]"
- User asks for statistics, trends, or data on a subject
- User says "I need to understand [topic] before making a decision"
- User says "compile research on [topic]"
- User says "intelligence report on [topic]"
- User says "dig into [topic]" or "investigate [topic]"
- User wants current, web-sourced information (not just AI knowledge)
- User needs data to make a business decision
- User asks "what do we know about [X]?"

## How It Works

### Step 1: Define the Research Scope

Parse the user's request to determine:

1. **Core Topic:** The primary subject to research
2. **Research Angle:** What perspective matters? (market size, competitors, trends, how-to, risks, opportunities)
3. **Depth Level:** Quick brief (300 words, 3-5 bullets) / Standard (1-2 pages) / Deep dive (5-10 pages)? Default to saved preference.
4. **Decision Context:** Why does the user need this? What decision will it inform?
5. **Time Sensitivity:** Is current data critical, or is evergreen info acceptable?

If the user did not specify depth, use their saved preference. If no preference is saved, default to Standard.

### Step 2: Build the Research Framework

Structure the investigation around these dimensions (include all that are relevant):

| Dimension | What to Find |
|-----------|-------------|
| Market Overview | Size, growth rate, key players, TAM/SAM/SOM |
| Current State | What is happening right now in this space |
| Key Trends | Where is this heading? What is changing? |
| Major Players | Who are the leaders? What are they doing? |
| Data & Statistics | Hard numbers, percentages, dollar figures |
| Opportunities | Gaps, underserved segments, whitespace |
| Risks & Challenges | What could go wrong? What are the barriers? |
| Expert Opinions | What are thought leaders saying? |
| Case Studies | Real examples of success or failure |
| Actionable Takeaways | What should the user DO with this information? |

### Step 3: Execute Web Research

Use the WebSearch tool to run multiple search queries. Do NOT rely on a single search. Run at least 5-8 targeted searches to build a complete picture:

**Search Strategy:**
1. **Broad overview search:** "[topic] overview 2026" or "[topic] market size"
2. **Statistics search:** "[topic] statistics data 2025 2026"
3. **Trend search:** "[topic] trends 2026" or "future of [topic]"
4. **Player/competitor search:** "top [topic] companies" or "[topic] market leaders"
5. **Problem/challenge search:** "[topic] challenges risks"
6. **Opportunity search:** "[topic] opportunities gaps underserved"
7. **Expert opinion search:** "[topic] expert analysis" or "[topic] thought leadership"
8. **Recent news search:** "[topic] news latest"

For each search, extract:
- Key facts and figures (with source URLs)
- Quotes or claims from credible sources
- Data points that support or challenge assumptions

Use WebFetch to pull deeper detail from particularly valuable pages when a search result looks promising but needs more context.

### Step 4: Synthesize Findings

Do NOT just list search results. Synthesize the information into a coherent narrative:

1. **Cross-reference data points** — If multiple sources cite different numbers, note the range and cite both
2. **Identify patterns** — What themes emerge across multiple sources?
3. **Separate facts from opinions** — Label which is which
4. **Highlight contradictions** — If sources disagree, surface the tension
5. **Connect to user's context** — How does this relate to their business, goals, or decision?

### Step 5: Compile the Research Document

**Quick Brief format** (for quick depth):
```markdown
# Research Brief: [Topic]
> Date: YYYY-MM-DD | Depth: Quick Brief

## Key Takeaways
1. [Most important finding]
2. [Second finding]
3. [Third finding]
4. [Fourth finding]
5. [Fifth finding]

## Bottom Line
[2-3 sentences: what this means and what to do with it]

## Sources
- [Source](URL)
```

**Standard / Deep Dive format:**
```markdown
# Research: [Topic Title]

> **Date:** YYYY-MM-DD
> **Purpose:** [What decision or understanding this serves]
> **Depth:** Standard / Deep Dive

---

## Executive Summary

[3-5 bullet points capturing the most important findings. A busy person should be able to read ONLY this section and get 80% of the value.]

---

## Market Overview

[Size, scope, growth trajectory. Hard numbers.]

## Current Landscape

[What is happening right now. Key players, recent developments.]

## Key Trends

[Where this is heading. What is changing. Emerging patterns.]

## Data & Statistics

| Metric | Value | Source |
|--------|-------|--------|
| [Market size] | [$X billion] | [Source name + URL] |
| [Growth rate] | [X% CAGR] | [Source name + URL] |
| [Key stat] | [Value] | [Source name + URL] |

## Opportunities

[Gaps in the market. Underserved segments. Whitespace.]

## Risks & Challenges

[What could go wrong. Barriers to entry. Headwinds.]

## Competitive Landscape

| Player | What They Do | Strength | Weakness |
|--------|-------------|----------|----------|
| [Company] | [Description] | [Strength] | [Weakness] |

## Expert Perspectives

[What credible voices are saying. Named quotes with attribution.]

## Actionable Takeaways

1. [What should the user DO based on this research]
2. [Specific next step]
3. [Strategic recommendation]

---

## Sources

1. [Source Title](URL) — accessed YYYY-MM-DD
2. [Source Title](URL) — accessed YYYY-MM-DD

---

*Research compiled by the provider AI. Verify critical data points before making major decisions.*
```

### Step 6: Save the Document

Save to cloud brain using `mcp__cloud-brain__write_note` with:
- `--title` "{topic-slug}" (e.g., "electric-vehicle-market-research")
- `--folder` "brain/research"
- `--tags` "research, deep-dive, {topic keywords}"

Naming convention:
- Use kebab-case for the title
- Include the subject, not the date (date is in the document header)
- Check for existing note first using `mcp__cloud-brain__search_notes` — if found, ask: "A research note on this topic already exists (dated {date}). Update it or create a new version?"

### Step 7: Report to User

Present:
1. The executive summary (key takeaways)
2. The most surprising or actionable finding
3. Where the full document was saved
4. Any gaps in the research (and suggestions for how to fill them)

## Research Quality Standards

1. **Cite everything.** Every data point gets a source. No unattributed claims.
2. **Current data preferred.** 2025-2026 data beats 2023 data. Flag when only older data is available.
3. **Multiple sources.** Never rely on a single source for a key claim. Cross-reference.
4. **Separate fact from opinion.** If a consultant says the market will grow 20%, that is an opinion. If revenue data shows 20% growth, that is a fact.
5. **Actionable output.** End with "so what?" — what should the user DO with this information.
6. **Acknowledge gaps.** If you could not find data on something, say so. Do not fabricate statistics.
7. **No fluff.** Every paragraph should contain information density. Cut filler.

## Example Usage

**User:** "Research the subscription software market for small businesses"

**AI executes:**
- 8+ web searches across market size, players, trends, regulations, opportunities
- Synthesizes into a structured document calibrated to saved depth preference
- Saves to cloud brain (folder: brain/research, title: smb-subscription-software-research)
- Reports executive summary + location

**User:** "Quick brief on AI agent platforms — who are the competitors?"

**AI executes:**
- Focused searches on AI agent platforms, pricing, funding, key players
- Delivers 5-bullet Quick Brief (300 words)
- Saves to cloud brain

**User:** "What's the market for online course platforms in 2026?"

**AI executes:**
- Market sizing, growth data, key players, pricing comparison, feature analysis
- Trend analysis (AI integration, community features, cohort models)
- Standard depth unless preference saved otherwise

## Error Handling

- **If WebSearch is unavailable:** Inform the user: "Web search is currently unavailable. I can still provide analysis based on my training data (up to my knowledge cutoff), but results will not include the latest data. Want me to proceed, or try again later?"
- **If web search returns no results for a specific query:** Try 2-3 alternative query phrasings. If still no results, note the gap: "Data not found for: {query topic}. This may require manual research or access to a paid industry database."
- **If the user does not specify a topic:** Ask: "What topic do you want me to research? The more specific you are, the better — 'AI agent platforms for small business' is better than just 'AI'."
- **If sources contradict each other on key data points:** Present both with sources: "Sources disagree on this metric — {Source A} reports {X} while {Source B} reports {Y}. Cross-reference before using these figures for critical decisions."
- **If the research topic is too broad:** Ask: "Can you narrow that down? For example: 'AI SaaS market size', 'EV charging station trends', or 'creative finance regulation changes in 2026'."
- **If WebFetch fails on a specific URL:** Skip and note: "Could not access {URL} for deeper detail. The search snippet has been included instead." Continue with other sources.
