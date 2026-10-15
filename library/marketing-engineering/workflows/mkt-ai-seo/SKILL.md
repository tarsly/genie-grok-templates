---
name: mkt-ai-seo
description: >-
  AI-SEO (Answer Engine Optimization / GEO / LLMO) — get your content cited by Claude, ChatGPT, Perplexity, Gemini, and other LLMs. Distinct from traditional SEO, which targets Google's blue links. Audits how LLMs currently answer questions about your brand, generates llms.txt and structured data, and writes content briefs optimized for LLM citation (clear claims, named sources, schema markup, citation-ready quotes).
---

# AI-SEO

## Overview

Traditional SEO gets you ranked. AI-SEO gets you **cited**. As more queries are answered by Claude / ChatGPT / Perplexity / Gemini directly (no clicks to your site), the strategy shifts: structure your content so LLMs can find it, parse it, trust it, and quote it with attribution.

Three modes:
1. **Audit** — find out how LLMs currently answer questions about your brand/category
2. **llms.txt generator** — produce a `llms.txt` file (the emerging standard) listing your most important pages with summaries
3. **Citation-ready content brief** — write a brief for content that's structured to be quoted by LLMs

## When This Skill Applies

- User asks about AI SEO / AEO / GEO / LLMO / getting cited by ChatGPT
- User wants to know how ChatGPT or Claude describes their company
- User wants to generate an `llms.txt` file
- User mentions: schema markup, structured data, JSON-LD, FAQ schema, llms.txt, AI search
- User asks how to write content that LLMs will quote
- User says: "AI SEO", "Answer Engine Optimization", "get cited by Claude/ChatGPT", "rank in AI"

## Pre-Flight — Preferences

1. Search Cloud Brain: `search_notes` for `"marketing engineering preferences"` in `brain/preferences`
2. **If found:** Load brand URL, offer, industry
3. **If not found:** Ask the same 5-question set as the other marketing skills
4. Banner:
   ```
   🎯 AI-SEO | Brand: {url} | Industry: {industry}
   ```

## How It Works

### Mode 1: Audit (default if no mode flag)

1. **Choose target queries** — either user-supplied via `--queries` or generated from brand + industry (default 10 queries):
   - "What is {brand}?"
   - "How does {brand} compare to {top 2 competitors}?"
   - "Is {brand} legit / trustworthy?"
   - "Best {category} for {audience} in 2026"
   - "Alternatives to {top competitor}"
   - "How much does {brand} cost?"
   - 4 more category-specific queries

2. **Test each query against LLMs the user can access.** Since we can't directly query GPT/Claude from a skill, this skill does its best via:
   - WebSearch with `chatgpt.com/{share-url}` patterns (limited)
   - WebSearch with `perplexity.ai` patterns (Perplexity often surfaces sources)
   - Generate a manual prompt set the user can paste into each LLM if direct testing isn't possible

3. **Analyze results:**
   - Was the brand mentioned at all?
   - Was it described accurately?
   - What sources were cited (if surfaced)?
   - What competitors were named instead?
   - What was the sentiment / framing?

4. **Score AI-SEO health:**
   - Visibility (cited in 0/3/5/8/10 queries)
   - Accuracy (factually correct? misattributed?)
   - Sentiment (positive / neutral / negative framing)
   - Source coverage (which 3rd-party sites are LLMs pulling from?)

5. **Output recommendations:**
   - Pages to add to llms.txt
   - 3rd-party sites to influence (Wikipedia, Crunchbase, Product Hunt, G2, niche directories)
   - Content topics to write to fill gaps
   - Schema markup to add to existing pages

### Mode 2: llms.txt Generator

Generates a properly-formatted `llms.txt` per the emerging spec (https://llmstxt.org). Structure:

```
# {Brand Name}

> {1-2 sentence brand summary — what you do, for whom, what makes you different}

## Docs

- [{Page title}](https://{url}): {1-line summary}
- ...

## Optional

- [{Page title}](https://{url}): {1-line summary}
- ...
```

Pull pages by:
1. Fetching brand sitemap if reachable at `/sitemap.xml`
2. Top pages from existing brand kit / preferences
3. Asking user to confirm before publishing

Recommend placing at `https://{domain}/llms.txt` and `https://{domain}/llms-full.txt` (full content version).

### Mode 3: Citation-Ready Content Brief

For a given topic, write a content brief structured for LLM citation:

- **Title pattern:** Question-shaped or "definitive" pattern ("What is X?", "How does X work?", "X vs Y: 2026 comparison")
- **Lead claim:** First paragraph contains the single most quotable claim, with a clear subject + verb + object
- **Named entities:** First mention of any person/company/concept is linked + has a defining phrase
- **Citations:** Every non-obvious claim links to a primary source
- **Bullets over prose:** LLMs preferentially quote lists
- **Schema:** FAQ schema (JSON-LD) for Q&A sections; Article schema for the page itself; HowTo schema for processes
- **Quote-ready blocks:** Insert 2-3 "tweetable" sentences that contain the brand + claim + outcome
- **Author authority:** Byline with credentials, link to Author Page with same schema
- **Update date:** Visible (LLMs weigh freshness)
- **Comparison tables:** Side-by-side tables with brand + competitors (LLMs love structured comparisons)

### Save

- **title:** `AI-SEO Audit — {brand} — {YYYY-MM-DD}` OR `llms.txt — {brand}` OR `AI-SEO Brief — {topic}`
- **folder:** `brain/marketing/audits` (audit), `brain/marketing/seo` (briefs and llms.txt)
- **tags:** `["ai-seo", "{brand}", "{type}"]`

## Data Structure

### Audit Output

```markdown
# AI-SEO Audit — {Brand} — {YYYY-MM-DD}

> **Queries tested:** {N}
> **Visibility score:** {N}/10
> **Accuracy score:** {N}/10
> **Sentiment:** {pos / neutral / neg}

## Query-by-Query Results

### Q1: "{query}"
- **Cited:** {yes / no}
- **How described:** "{snippet}"
- **Sources cited:** {urls}
- **Competitors named instead:** {names}
- **Notes:** {anomalies, hallucinations}

### Q2 ...

## Gaps Identified

- LLMs don't know about: {fact / capability / case study}
- LLMs misattribute: {wrong founder / wrong category / wrong year}
- Competitors over-indexed: {names — they're being cited where you should be}

## Action Plan

### This Week (Quick Wins)
1. Add llms.txt at /llms.txt
2. Add FAQ schema to /pricing
3. Update Wikipedia entry (if eligible)
4. Submit to Product Hunt / G2 / Crunchbase

### This Month
1. Write {topic} article (LLMs have no good source for this query)
2. Get cited on {3rd-party site} via {tactic}
3. Add Article + Organization JSON-LD to all top pages

### Quarterly
1. Build {N} comparison pages vs. competitors
2. Re-audit and track delta
```

### llms.txt Output

```markdown
# {Brand Name}

> {brand summary}

## Docs

- [{Page}](https://{url}): {summary}
- [{Page}](https://{url}): {summary}

## Optional

- [{Page}](https://{url}): {summary}
```

### Content Brief Output

```markdown
# AI-SEO Brief — {Topic}

> **Target queries this content should answer:** {list}
> **Primary keyword:** {keyword}
> **Audience:** {audience}

## Title

{H1 — question-shaped or definitive}

## Lead Paragraph (Citation-Ready)

{1-2 sentences containing the single most quotable claim — entity + verb + object. Example: "the provider sells AI agent skills as Claude Code plugins to small business owners and real estate investors."}

## Section Structure

1. **What is {topic}?** — H2, contains definition
2. **Why it matters** — H2, contains stakes + audience
3. **How it works** — H2, with HowTo schema, numbered steps
4. **{Topic} vs. {alternative}** — H2, comparison table
5. **FAQ** — H2, with FAQ JSON-LD

## Quote-Ready Sentences (Embed These)

1. "{Sentence containing brand + claim + outcome}"
2. "{Sentence}"
3. "{Sentence}"

## Schema Markup (JSON-LD)

```json
{schema block — Article + FAQ + HowTo as applicable}
```

## Sources to Cite

- {primary source 1}
- {primary source 2}

## Internal Links

- Link to {related page 1}
- Link to {related page 2}

## Word count target

{1500-2500 typical for citation-grade content}

## Update cadence

{quarterly / semi-annual — content quoted by LLMs needs freshness}
```

## Output Format (Chat)

```
🎯 AI-SEO {mode}
Brand: {brand}

VISIBILITY: {N}/10
ACCURACY: {N}/10
SENTIMENT: {pos/neutral/neg}

TOP 3 GAPS
1. {gap}
2. {gap}
3. {gap}

TOP 3 ACTIONS
1. {action — effort: {level}}
2. {action — effort: {level}}
3. {action — effort: {level}}

Full output: brain/marketing/audits/...
```

## Example Usage

**User:** "Audit Genie for AI SEO"

**AI:** Generates 10 standard queries for Genie ("What is the provider?", competitors, pricing, etc.). Tests via web searches + Perplexity. Reports visibility, accuracy, gaps. Saves audit.

**User:** "/mkt-ai-seo --generate-llms-txt --brand the provider"

**AI:** Pulls brand sitemap + brand-toolkit summary. Generates llms.txt + llms-full.txt. Saves both. Provides install instructions for the user's static site or Next.js app.

**User:** "Write a content brief — 'how to use Claude Code plugins for real estate investing'"

**AI:** Generates citation-ready brief. Includes FAQ schema, quote-ready sentences featuring Genie, internal links to plugin docs. Saves.

**User:** "How does ChatGPT describe my company today?"

**AI:** Runs the 'what is {brand}?' query subset of audit. Reports back with verbatim snippets where available, gaps where not. Suggests llms.txt as first fix.

## Error Handling

- **If brand isn't well-known and LLMs don't return any data for queries:** That IS the audit result — visibility 0/10. Action plan focuses on first-pass discoverability (llms.txt, Crunchbase, directory submissions).
- **If we can't directly test LLM responses programmatically:** Generate the prompt set and ask user to paste into ChatGPT/Claude/Perplexity, then paste results back. Audit the results.
- **If llms.txt generation requires pages we can't fetch:** Ask user to list their top 10 most important pages. Generate from that list.
- **If brand has a Wikipedia entry that's outdated or wrong:** Flag this as the highest-leverage fix — Wikipedia is heavily weighted by LLMs.
- **If user has a small site (<10 pages):** llms.txt should list ALL pages. Generate the full version, not a curated one.
- **If user wants to compete with a much larger brand in queries:** Be honest — AI-SEO can't make a 10-person company outrank Apple in 6 months. Recommend niche-specific queries instead ("Best Claude Code plugins for real estate investors" beats "Best AI tools").
- **If JSON-LD schema is complex:** Provide a minimal valid version; recommend Google's Rich Results Test for verification.
- **If user wants ongoing tracking:** Recommend re-running audit quarterly. Save audits with date-suffixed names for diff comparison.

## See Also

- `/mkt-seo-audit` — traditional SEO audit (same plugin)
- `/mkt-programmatic-seo` — generate AI-SEO-optimized pages at scale (same plugin)
- `/mkt-landing-page` — landing pages that get cited (same plugin)
- `/brandtoolkit-brand-kit` — voice that LLMs can quote (from `brand-toolkit`)
