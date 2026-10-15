---
name: research-swot
description: >-
  Strategic SWOT analysis — strengths, weaknesses, opportunities, threats, risk analysis, pros and cons, GO/NO-GO verdict for any business decision, market opportunity, partnership, investment, or competitive position.
---

# SWOT Analysis — Strategic Intelligence Assessment

## Overview

SWOT Analysis produces a comprehensive strategic assessment for any business decision, opportunity, market, or initiative. It goes far beyond a basic four-quadrant chart. Each section includes evidence-based analysis, specific examples, and concrete action items. The final output includes a strategic recommendation with a clear GO/NO-GO/CONDITIONAL verdict. Designed for entrepreneurs who need structured strategic thinking fast — customized to their exact situation, not a generic template.

## When This Skill Applies

- User says "SWOT analysis" or "SWOT" or "strengths weaknesses opportunities threats"
- User says "strategic analysis" or "strategic assessment" or "strategic review"
- User says "analyze this opportunity" or "evaluate this business" or "evaluate this market"
- User says "should I do this" or "is this a good idea"
- User says "what are the risks" or "what could go wrong" or "pros and cons"
- User says "risk analysis" or "opportunity analysis" or "market assessment"
- User says "analyze this decision"
- User is weighing a significant business decision and needs structured thinking

## How It Works

### Step 1: Define the Subject

Clarify exactly what's being analyzed. Ask the user (if not already provided):

1. **What are we analyzing?** — A business idea, market entry, partnership, product, investment, hire, pivot, expansion, etc.
2. **What's the context?** — Current position, resources, timeline, constraints
3. **What decision does this inform?** — Go/no-go, invest/pass, enter/avoid, build/buy, etc.
4. **Who are the key competitors or alternatives?**

If the user gives a one-liner like "SWOT on starting a YouTube channel," that's enough to start. Don't ask 10 questions.

### Step 2: Research (If Web Search Available)

- Search for market data, industry reports, and trends related to the subject
- Research competitors and their positioning
- Look for recent news, shifts, or disruptions in the space
- Find relevant statistics (market size, growth rates, failure rates)
- Check for regulatory or legal considerations

If web search is unavailable, work with the user's context and general domain knowledge. Note where additional research would strengthen the analysis.

### Step 3: Check Cloud Brain

- Use `mcp__cloud-brain__search_notes` with keyword "goals" — Does this align with the user's stated vision and priorities?
- Use `mcp__cloud-brain__search_notes` with keyword "project" — Does this conflict with or complement active projects?
- Use `mcp__cloud-brain__search_notes` with keywords related to the topic — Is there existing research or a prior SWOT to build on?

If a prior SWOT exists for this topic, surface it: "You ran a SWOT on this topic on {date}. Want me to update that analysis or build a fresh one?"

### Step 4: Build the SWOT

For each quadrant, identify 5-8 specific, evidence-based points. Not generic observations — specific, actionable intelligence tied to the user's situation.

**Strengths (Internal, Positive)**
- What advantages does the user/business have?
- What unique resources, skills, or assets exist?
- What's working well right now?
- What competitive edge exists?

**Weaknesses (Internal, Negative)**
- What's missing? Resources, skills, team, capital?
- Where is the user/business vulnerable?
- What patterns of failure or underperformance exist?
- What would a competitor exploit?

**Opportunities (External, Positive)**
- What market trends favor this move?
- What gaps exist in the market?
- What external changes create openings?
- What timing advantages exist right now?

**Threats (External, Negative)**
- What competitors are in the way?
- What market/economic risks exist?
- What regulatory or legal risks?
- What could go wrong that's outside your control?

### Step 5: Cross-Quadrant Analysis

This is what separates strategic intelligence from a basic chart. Analyze the intersections:

- **Strength + Opportunity (SO Strategies):** How to use strengths to capture opportunities
- **Strength + Threat (ST Strategies):** How to use strengths to mitigate threats
- **Weakness + Opportunity (WO Strategies):** How to fix weaknesses to unlock opportunities
- **Weakness + Threat (WT Strategies):** How to address weaknesses before threats exploit them

### Step 6: Action Items

For each quadrant, generate 2-3 specific, time-bound action items:
- **Leverage** — Actions to capitalize on strengths
- **Fix** — Actions to address weaknesses
- **Capture** — Actions to seize opportunities
- **Defend** — Actions to mitigate threats

### Step 7: Strategic Recommendation

Deliver a clear verdict:
- **GO** — The opportunity is strong, risks are manageable, move forward
- **NO-GO** — Risks outweigh benefits, threats are too severe, pass on this
- **CONDITIONAL GO** — Proceed only if [specific conditions] are met first

Support the verdict with 2-3 sentences of reasoning. Don't soften a NO-GO.

### Step 8: Save the Analysis

Save to cloud brain using `mcp__cloud-brain__write_note` with:
- `--title` "swot-{topic-slug}-{YYYY-MM-DD}"
- `--folder` "brain/research"
- `--tags` "swot, analysis, strategy"

## Output Format

```markdown
# SWOT Analysis: [Subject]
> **Date:** [YYYY-MM-DD]
> **Decision:** [What decision this informs]
> **Verdict:** [GO / NO-GO / CONDITIONAL GO]

---

## Executive Summary

[3-5 sentences: what was analyzed, key finding, recommendation, and the single biggest factor in the decision]

---

## Context

[Brief description of the subject, the user's current position, and why this analysis was requested]

---

## STRENGTHS (Internal, Positive)

| # | Strength | Impact | Evidence |
|---|----------|--------|----------|
| 1 | [Specific strength] | [High/Med/Low] | [Why this is real] |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |

**Key Strength:** [The single biggest advantage and how to exploit it]

### Action: Leverage These Strengths
- [ ] [Specific action — by when]
- [ ] [Specific action]
- [ ] [Specific action]

---

## WEAKNESSES (Internal, Negative)

| # | Weakness | Severity | Mitigation |
|---|----------|----------|-----------|
| 1 | [Specific weakness] | [High/Med/Low] | [What can be done] |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |

**Critical Weakness:** [The one that could sink this if not addressed]

### Action: Fix These Weaknesses
- [ ] [Specific action — by when]
- [ ] [Specific action]
- [ ] [Specific action]

---

## OPPORTUNITIES (External, Positive)

| # | Opportunity | Timeline | Probability |
|---|-------------|----------|-------------|
| 1 | [Specific opportunity] | [When available] | [High/Med/Low] |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |

**Biggest Opportunity:** [The one with the highest upside and how to capture it]

### Action: Capture These Opportunities
- [ ] [Specific action — by when]
- [ ] [Specific action]
- [ ] [Specific action]

---

## THREATS (External, Negative)

| # | Threat | Severity | Probability | Early Warning |
|---|--------|----------|-------------|---------------|
| 1 | [Specific threat] | [High/Med/Low] | [Likely/Possible/Unlikely] | [Signal to watch] |
| 2 | | | | |
| 3 | | | | |
| 4 | | | | |
| 5 | | | | |

**Biggest Threat:** [The one most likely to cause damage and how to defend]

### Action: Defend Against These Threats
- [ ] [Specific defensive action — by when]
- [ ] [Specific action]
- [ ] [Specific action]

---

## Cross-Quadrant Strategies

### SO: Use Strengths to Capture Opportunities
- [Strategy: How strength X positions you to seize opportunity Y]
- [Strategy 2]

### ST: Use Strengths to Counter Threats
- [Strategy: How strength X defends against threat Y]
- [Strategy 2]

### WO: Fix Weaknesses to Unlock Opportunities
- [Strategy: Addressing weakness X would unlock opportunity Y]
- [Strategy 2]

### WT: Shore Up Weaknesses Before Threats Exploit Them
- [Strategy: Weakness X is most dangerous because of threat Y — fix this first]
- [Strategy 2]

---

## Risk Matrix

| Risk | Probability | Impact | Priority | Response |
|------|------------|--------|----------|----------|
| [Risk 1] | [H/M/L] | [H/M/L] | [1-5] | [Accept/Mitigate/Avoid/Transfer] |
| [Risk 2] | | | | |
| [Risk 3] | | | | |

---

## Strategic Recommendation

### Verdict: [GO / NO-GO / CONDITIONAL GO]

[2-3 paragraph analysis supporting the verdict. What tips the balance? What's the biggest risk? What's the cost of inaction? What conditions would change the recommendation?]

### If GO: Next Steps
1. [First thing to do — this week]
2. [Second priority — this month]
3. [Third priority — this quarter]

### If NO-GO: What Would Change the Answer
- [Condition 1 that would make this a GO]
- [Condition 2]

### If CONDITIONAL: Required Conditions
- [ ] [Condition 1 — must be true before proceeding]
- [ ] [Condition 2]
- [ ] [Condition 3]

---

*Analysis completed: [YYYY-MM-DD] by the provider AI. Review quarterly or when conditions change significantly.*
```

## Error Handling

- **If the user's request is too vague:** Ask one clarifying question: "What specifically should I analyze? A business idea, a market, a decision you're weighing?" Don't ask 10 questions — get enough to start.
- **If web search is unavailable:** Produce the analysis using user context and general knowledge. Label assumptions clearly: "Based on general market knowledge — verify with current data." The analysis is still valuable.
- **If the subject conflicts with the user's stated goals:** Flag it prominently: "⚠️ Note: This opportunity appears to conflict with your stated priority of [X]. Proceeding would split focus — factor this into the decision."
- **If the analysis reveals a clear NO-GO:** Don't soften it. "This is a no-go. Here's why." Honest analysis is more valuable than validation.
- **If the analysis is genuinely balanced:** Use CONDITIONAL GO and list the specific conditions that would tip it. Don't manufacture a verdict.

## Example Usage

**User:** "SWOT on launching a YouTube channel focused on real estate education"

**AI:**
1. Checks cloud brain for prior goals notes and any existing research on the topic
2. Searches web for: real estate YouTube channels, audience size, monetization, competition, content gaps
3. Builds full SWOT with the user's specific strengths (domain expertise, existing brand) and weaknesses (no existing audience, time constraints)
4. Cross-quadrant: expertise + market gap = strong opportunity; time constraint + competitive space = needs a content batching system
5. Verdict: CONDITIONAL GO — proceed only if a batched content workflow is built first
6. Saves to cloud brain (folder: brain/research)

**User:** "Should I hire a full-time sales rep or use a commission-only contractor?"

**AI:**
1. Checks cloud brain for business context and financial notes
2. SWOT on each option side-by-side
3. Cross-quadrant analysis highlighting how cash position and sales cycle length affect the decision
4. Delivers CONDITIONAL verdict with specific financial benchmarks that determine which path to take
