# Research Intelligence Plugin
### by the provider | v1.0.0

A suite of four AI-powered research skills that turn any business question into a structured, sourced intelligence document — saved to your cloud brain and personalized to your business over time.

---

## What's Included

| Skill | What It Does | Best For |
|-------|-------------|----------|
| **Deep Research** | Comprehensive investigation of any topic — statistics, trends, key players, opportunities | Understanding a space before making a decision |
| **Market Research** | TAM/SAM/SOM analysis, growth rates, competitive landscape, market entry assessment | Validating a market or sizing an opportunity |
| **Competitor Analysis** | Full competitive landscape — profiles, pricing, feature matrix, positioning map, battle cards | Knowing your competitive position and how to win |
| **SWOT Analysis** | GO/NO-GO/CONDITIONAL strategic assessment with cross-quadrant strategies and action items | Evaluating any business decision with structured thinking |
| **Person Research** | Individual due diligence profile — background, recent activity, conversation starters, red flags | Preparing for a sales call, investor pitch, partner meeting, or hire |

---

## How to Trigger Each Skill

### Deep Research
> "Research [topic]"
> "Deep dive on [topic]"
> "Give me a breakdown of [industry]"
> "Intelligence report on [topic]"
> "I need to understand [topic] before making a decision"

### Market Research
> "How big is the market for [X]?"
> "TAM/SAM/SOM for [industry]"
> "Is there a market for [product idea]?"
> "Market analysis for [niche]"
> "Market trends in [industry] for 2026"

### Competitor Analysis
> "Competitor analysis for [my product/business]"
> "Who are my competitors in [market]?"
> "How do we stack up against [competitor]?"
> "Competitive landscape for [space]"
> "Give me a battle card for us vs. [competitor]"

### SWOT Analysis
> "SWOT analysis on [topic/decision]"
> "Should I [do this thing]?"
> "Strategic assessment of [opportunity]"
> "What are the risks of [decision]?"
> "Pros and cons of [option]"
> "Analyze this opportunity: [description]"

### Person Research
> "Research [full name] before my meeting"
> "Who is [name] at [company]?"
> "Prep me for a call with [name]"
> "Due diligence on [name]"
> "Give me a brief on [name] — I'm pitching them tomorrow"

---

## First Run — Preferences Setup

On first use, each skill will ask you a few setup questions to personalize results:
- Your business name and what it does
- Your primary industry and markets
- Default research depth (Quick Brief / Standard / Deep Dive)
- Geography defaults for market sizing
- Known competitors to always include

Preferences are saved to your cloud brain and applied automatically on every future run. To update them, say: "Update my [skill] preferences."

---

## Two Skills That Work Together

**Market Research → SWOT Analysis**
Run market research first to size an opportunity, then run a SWOT to decide whether to pursue it. The SWOT skill automatically checks your cloud brain for prior research on the topic.

**Competitor Analysis → SWOT Analysis**
Run competitor analysis to map the landscape, then feed those findings into a SWOT for a strategic positioning decision.

**Person Research → [Any meeting]**
Run person research before any high-stakes meeting. Profiles are saved to your cloud brain under `brain/people/` and retrievable for future interactions with the same person.

---

## Output & Storage

All research is saved to your cloud brain:
- Deep Research → `brain/research/[topic-slug]`
- Market Research → `brain/research/[topic-slug]-market-research`
- Competitor Analysis → `brain/research/[market]-competitive-analysis`
- SWOT Analysis → `brain/research/swot-[topic-slug]-[date]`
- Person Profiles → `brain/people/person-[name-slug]`

Results are retrievable in future sessions — run a follow-on analysis and the skill will find your prior work automatically.

---

## What's Coming in v1.1

- **Industry Brief** — 1-page cheat sheet on any industry for quick context before a meeting
- **Trend Watch** — Scheduled monitoring of topics that matter to your business, delivered as a digest

---

## Disclaimer

Research outputs are compiled from publicly available web sources for informational and planning purposes. All data should be independently verified before making major business decisions. Person profiles are based on public information only.

---

*the provider Research Intelligence Plugin v1.0.0*


## Grok Bot import
See the root README. Replace `{{BOT_NAME}}` in `system-prompt.md` and `workflows/getting-started/SKILL.md`.
