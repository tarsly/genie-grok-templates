---
name: research-competitor
description: >-
  Competitor analysis — research competitors, pricing, features, gaps, market positioning, battle cards, competitive audit, landscape analysis, or needs pricing comparisons, a feature gap analysis, a battle card, or a competitive positioning summary.
---

# Competitor Analysis — Competitive Landscape Intelligence

## Overview

Competitor Analysis performs a structured investigation of the competitive landscape for any business, product, or market. It identifies direct and indirect competitors, maps their pricing, features, positioning, strengths, and weaknesses, and produces an actionable competitive intelligence document. The output highlights gaps the user can exploit, threats to watch, and strategic positioning recommendations. Everything is sourced from current web data.

## When This Skill Applies

- User says "analyze my competitors" or "competitor analysis"
- User says "who are my competitors?" or "who else does this?"
- User says "competitive landscape" or "competitive research"
- User says "how do we stack up against [competitor]?"
- User says "competitor pricing" or "what do they charge?"
- User says "battle card" or "competitive intel"
- User mentions a specific competitor and wants to understand them
- User is launching a product and needs to know the landscape
- User asks "what makes us different?" or "what's our edge?"
- User says "find competitors for [product/service]"

## How It Works

### Step 1: Define the Competitive Frame

Determine from the user's request:

1. **The Business/Product:** What is being analyzed?
2. **The Market:** What space does it operate in?
3. **Known Competitors:** Has the user already named specific competitors?
4. **Analysis Depth:** Quick overview (top 5) or comprehensive (10-15+ with deep feature comparison)?
5. **Focus Area:** All-around analysis or focused on pricing, features, marketing, or positioning?

Check cloud brain for existing competitive intelligence:
- Use `mcp__cloud-brain__search_notes` with keywords like "competitor" or "competitive analysis"
- If prior research exists, surface it: "I found a prior competitive analysis from {date}. Want me to update it with fresh data, or run a fresh analysis?"

### Step 2: Identify Competitors

Run targeted web searches to find competitors across three tiers:

**Tier 1 — Direct Competitors** (same product/service, same target customer)
- Search: "[product type] platforms" or "[service] companies"
- Search: "[product name] alternatives" or "[product name] competitors"
- Search: "best [product category] 2026"

**Tier 2 — Indirect Competitors** (different approach, same customer problem)
- Search: "[customer problem] solutions"
- Search: "how to [what the product does] without [product type]"

**Tier 3 — Emerging/Adjacent Competitors** (entering the space or could pivot in)
- Search: "[industry] startups 2025 2026"
- Search: "[adjacent industry] expanding into [target market]"

For each identified competitor, note:
- Company name and URL
- One-line description of what they do
- Why they are a competitor (direct, indirect, or emerging)

### Step 3: Research Each Competitor

For the top 5-10 competitors, investigate:

| Data Point | How to Find It |
|-----------|---------------|
| **Pricing** | Search "[company] pricing", visit their pricing page via WebFetch |
| **Features** | Search "[company] features", check their product pages |
| **Target Market** | Who do they market to? Check their homepage copy and about page |
| **Positioning** | What is their tagline/value prop? How do they describe themselves? |
| **Strengths** | What do reviews and users praise? Search "[company] reviews" |
| **Weaknesses** | What do users complain about? Search "[company] complaints" or "[company] vs" |
| **Funding/Scale** | Search "[company] funding" or "[company] revenue" for size context |
| **Marketing Approach** | Check their social media presence, content strategy, ad spend clues |
| **Technology** | What tech stack or approach? Any unique technical advantages? |
| **Recent Moves** | Search "[company] news 2026" for recent product launches or changes |

### Step 4: Build Comparison Frameworks

Create three core comparison assets:

#### A. Feature Comparison Matrix

```markdown
| Feature | Your Product | Competitor A | Competitor B | Competitor C |
|---------|-------------|-------------|-------------|-------------|
| Feature 1 | Yes | Yes | No | Partial |
| Feature 2 | Yes | No | Yes | Yes |
| Pricing (entry) | $X/mo | $Y/mo | $Z/mo | Free |
| Target Market | [Desc] | [Desc] | [Desc] | [Desc] |
| Unique Feature | [What] | [What] | [What] | [What] |
```

#### B. Positioning Map

Describe where each competitor sits along two key dimensions relevant to the market (e.g., price vs. feature depth, consumer vs. enterprise, self-serve vs. full-service):

```
High Price
    |
    |  [Enterprise Player A]     [Premium Player B]
    |
    |         [Mid-Market C]
    |
    |  [Budget Player D]    [YOUR PRODUCT — positioned here]
    |
Low Price
    Self-Serve ————————————————————————— Full-Service
```

#### C. SWOT for Each Major Competitor

For the top 3-5 direct competitors:

```markdown
### [Competitor Name]
| | Helpful | Harmful |
|---|---------|---------|
| Internal | **Strengths:** [list] | **Weaknesses:** [list] |
| External | **Opportunities:** [for them] | **Threats:** [to them] |
```

#### D. Battle Card (Optional — request with "give me a battle card")

A condensed 1-page competitive comparison for use in sales conversations:

```markdown
## Battle Card: [Your Product] vs. [Competitor]

| | You Win | They Win |
|---|---------|---------|
| Price | | |
| Feature X | | |
| Feature Y | | |
| Support | | |
| Integrations | | |

**When you're losing to them:** [objection + response]
**Your killer differentiator:** [one sentence]
**Their biggest weakness to exploit:** [one sentence]
```

### Step 5: Identify Strategic Insights

Synthesize the research into actionable intelligence:

1. **Gaps in the Market:** What are competitors NOT doing that customers want?
2. **Your Competitive Advantages:** Based on the analysis, where does the user's product/service win?
3. **Threats to Watch:** Which competitors could become a problem? What moves could hurt the user?
4. **Pricing Insights:** Where is the user positioned relative to competitors? Is there a pricing opportunity?
5. **Positioning Recommendation:** How should the user differentiate? What messaging beats the competition?
6. **Quick Wins:** Tactical things the user can do right now to gain an edge

### Step 6: Compile the Document

```markdown
# Competitive Analysis: [Market/Product]

> **Date:** YYYY-MM-DD
> **Analyzed For:** [User's product/business name]
> **Market:** [The competitive space]
> **Competitors Analyzed:** [Count]

---

## Executive Summary

[3-5 bullet points. The most important takeaways for decision-making.]

---

## Competitive Landscape Overview

[1-2 paragraphs describing the overall competitive environment.]

## Competitor Profiles

### 1. [Competitor Name] — [One-line description]
- **URL:** [website]
- **Founded/Size:** [Year, employee count or funding if available]
- **What They Do:** [2-3 sentences]
- **Pricing:** [Tiers and prices]
- **Target Customer:** [Who they serve]
- **Strengths:** [Bullet list]
- **Weaknesses:** [Bullet list]
- **Key Differentiator:** [What makes them unique]
- **Threat Level:** Low / Medium / High

[Repeat for each competitor]

## Feature Comparison Matrix

[Table comparing key features across all competitors]

## Pricing Comparison

| Competitor | Entry Price | Mid Tier | Top Tier | Free Option |
|-----------|------------|---------|---------|-------------|
| [Name] | $X/mo | $Y/mo | $Z/mo | Yes/No |

## Positioning Map

[Visual or text description of where each competitor sits]

## Strategic Insights

### Market Gaps (Opportunities)
1. [Gap 1 — what no one is doing well]
2. [Gap 2]
3. [Gap 3]

### Your Competitive Advantages
1. [Advantage 1]
2. [Advantage 2]

### Threats to Watch
1. [Threat 1]
2. [Threat 2]

### Positioning Recommendation
[How should the user position against this landscape?]

### Quick Wins
1. [Tactical action the user can take immediately]
2. [Tactical action]
3. [Tactical action]

---

## Sources

1. [Source Title](URL) — accessed YYYY-MM-DD

---

*Competitive intelligence compiled by the provider AI. Markets shift — revisit this analysis quarterly.*
```

### Step 7: Save and Report

Save to cloud brain using `mcp__cloud-brain__write_note` with:
- `--title` "{product-or-market}-competitive-analysis"
- `--folder` "brain/research"
- `--tags` "competitor, analysis, research"

Present to the user:
- Executive summary (the key competitive takeaways)
- The biggest opportunity and biggest threat
- Where the document was saved
- Recommendation for how often to refresh this analysis (suggest quarterly)

## Quality Standards

1. **Real data, not assumptions.** Every competitor profile is based on what was actually found online, not guesses.
2. **Pricing verified.** If pricing is not publicly available, note "pricing not public — contact required" rather than guessing.
3. **Fair assessment.** Honest analysis is more useful than cheerleading. Do not inflate the user's advantages.
4. **Actionable.** The document must answer "so what do I DO with this information?"
5. **Current.** Prioritize 2025-2026 data. Flag if a competitor's info might be outdated.
6. **Sourced.** Every factual claim includes a source URL.

## Example Usage

**User:** "Competitor analysis for my project management SaaS targeting construction companies"

**AI executes:**
- Identifies direct competitors (Procore, Buildertrend, CoConstruct, etc.)
- Identifies indirect competitors (generic PM tools used by construction firms)
- Profiles 8-10 competitors with pricing, features, strengths, weaknesses
- Builds feature matrix and positioning map
- Identifies market gaps and differentiation opportunities
- Saves to cloud brain (folder: brain/research)

**User:** "Who are the competitors for an AI scheduling tool for medical offices?"

**AI executes:**
- Searches AI scheduling, medical office software, patient scheduling tools
- Profiles players across tiers (direct: Zocdoc, Acuity; indirect: generic scheduling tools)
- Compares pricing, target market, integrations
- Identifies positioning opportunities for a new entrant

**User:** "Give me a battle card for us vs. [Competitor]"

**AI executes:**
- Focuses research on one head-to-head comparison
- Produces condensed battle card format
- Highlights where to win and how to counter their objections

## Error Handling

- **If WebSearch is unavailable:** Inform the user and offer analysis from training data with a current-data caveat.
- **If web search returns no pricing data:** Note "pricing not public — contact required" rather than guessing.
- **If the user does not specify what to analyze:** Ask: "What product, service, or market do you want me to map the competitive landscape for?"
- **If no competitors can be found:** Report honestly: "I could not find direct competitors for this specific niche. This could mean: (1) whitespace opportunity, (2) the market uses different terminology, or (3) competitors have low online visibility. I identified {N} adjacent competitors instead." Then show indirect competitors.
- **If the user names a competitor that cannot be found online:** Note: "I couldn't find reliable information on '{competitor name}'. They may be very new, very small, or operating under a different name. Share a URL or more details and I'll dig deeper."
- **If WebFetch fails on a competitor's website:** Note in that profile: "Could not access {competitor}.com for detailed data. Analysis based on search snippets and third-party reviews." Continue with available data.
