---
name: sm-strategy-builder
description: >-
  Builds a complete social media strategy from discovery and brand data — platform prioritization, content pillar mix, posting frequency, optimal timing, and agent configuration recommendation. Use when: 'build my social media strategy', 'create a social media plan', 'update my strategy', 'what should I post and when', 'social media strategy', or as part of sm-onboard.
---

# SM Strategy Builder — Social Media Strategy & Platform Plan

## Overview

This skill reads the client's discovery profile and brand snapshot from Cloud Brain, then builds a complete social media strategy — platform priorities, content pillar allocation, posting frequency, optimal timing, and a recommendation for agent configuration. The strategy becomes the operating document for every other skill in the suite.

---

## Pre-Flight — Load Discovery & Brand Data

**Required before proceeding:**

1. `mcp__cloud-brain__search_notes` with query `"social media discovery"` — load discovery profile
2. `mcp__cloud-brain__search_notes` with query `"brand snapshot"` or `"brand audit"` — load brand profile

**If discovery data is missing:** Do not proceed. Tell the client: *"I need your discovery profile before building a strategy. Please run sm-discovery first."*

**If brand data is missing:** Proceed but note: *"I don't have your brand profile yet. I'll build the strategy framework, but run sm-brand-snapshot before creating content so your posts match your voice."*

**Check for existing strategy:**
`mcp__cloud-brain__search_notes` with query `"social media strategy"`

If a strategy exists and the client hasn't changed their discovery/brand data: *"I found an existing strategy from [date]. Would you like to review and update it, or build a fresh one?"*

---

## Step 1 — Platform Prioritization

Based on the discovery profile, rank the client's platforms by strategic priority. Use this framework:

**Tier 1 — Primary platforms** (post consistently, 4–7x/week, full content investment)
Choose 1–2 platforms based on:
- Where their target audience spends time
- Their content capacity (video-heavy? → TikTok/YouTube; text-heavy? → LinkedIn; visual? → Instagram)
- Where they already have traction or followers

**Tier 2 — Secondary platforms** (post 2–3x/week, often repurposed from Tier 1)
Choose 1–2 platforms. Content is adapted from Tier 1, not created from scratch.

**Tier 3 — Presence-only platforms** (minimal activity, profile maintained)
Any remaining platforms where they have accounts but shouldn't spread themselves thin.

**Platform guidance by audience type:**

| Audience | Primary platform | Secondary |
|---|---|---|
| B2C consumers, lifestyle | Instagram | TikTok, Facebook |
| B2B professionals | LinkedIn | Instagram |
| Young adults, Gen Z | TikTok | Instagram |
| Broad consumer (all ages) | Facebook | Instagram |
| Long-form content creators | YouTube | Instagram/TikTok |
| Local community / services | Facebook | Instagram |

Present the platform tier recommendation and ask for client confirmation before proceeding.

---

## Step 2 — Content Pillar Allocation

Using the content pillars defined in the brand snapshot, assign each pillar a posting allocation percentage.

Principles:
- One pillar should be **educational/value** (builds trust, gets saves/shares)
- One pillar should be **personal/story** (builds connection, gets comments)
- One pillar should be **promotional/sales** (drives action) — keep this under 20%
- Remaining pillars fill awareness and community goals

Example allocation for 5 pillars:
- Educational content: 30%
- Personal / behind-the-scenes: 25%
- Community / audience engagement: 20%
- Client wins / social proof: 15%
- Promotional / offers: 10%

Adapt these percentages to the client's goals (lead generation → more promotional; community building → more personal).

Present the pillar allocation and adjust based on client feedback.

---

## Step 3 — Posting Frequency & Timing

**Recommended posting frequency by platform (adjust down based on content capacity):**

| Platform | Minimum | Recommended | Content type |
|---|---|---|---|
| Instagram | 3x/week | 5x/week | Feed posts + Stories daily |
| TikTok | 3x/week | Daily | Short video |
| LinkedIn | 2x/week | 4x/week | Text posts, articles |
| Facebook | 3x/week | 5x/week | Posts + Stories |
| YouTube | 1x/week | 2x/week | Long-form or Shorts |

Adjust down based on the client's stated time availability and content capacity from discovery.

**Optimal timing:** If the Metricool MCP is connected, retrieve best posting times:
- `get_best_time_to_post` for each connected platform

If Metricool data is not yet available, use these research-backed defaults:
- Instagram: Tue–Fri, 9am–11am or 2pm–4pm (client's local time)
- TikTok: Mon–Fri, 7am–9am or 7pm–9pm
- LinkedIn: Tue–Thu, 8am–10am or midday
- Facebook: Wed–Sun, 1pm–4pm
- YouTube: Fri–Sun, 12pm–4pm

Note: *"Once your Metricool account has 30+ days of history, your agent will use your actual audience data instead of these defaults."*

---

## Step 4 — Content Type Mix

Recommend a content type mix per platform based on content capacity:

**If client can produce video:**
- 40% short-form video (Reels/TikTok/Shorts)
- 30% static graphics/carousels
- 20% text posts
- 10% Stories/ephemeral

**If client cannot produce video:**
- 50% static graphics/carousels
- 35% text posts
- 15% Stories/ephemeral

Note to client: *"If video feels out of reach right now, that's fine. Static graphics and strong writing can absolutely build an audience. You can add video later when you're ready."*

---

## Step 5 — Agent Configuration Recommendation

Based on the full profile, recommend one of these configurations:

**Single Social Media Manager Agent** (most clients)
Recommended when:
- 1–3 platforms active
- Solo operator or very small team
- Content is primarily graphics + text
- Time available for social media is limited

Description: *"One agent handles your full weekly workflow — trends, content creation, graphics, scheduling, and analytics."*

**Social Media Manager + Content Creator Agent** (content-heavy clients)
Recommended when:
- 4+ active platforms
- High posting frequency (daily or more)
- Strong video presence required
- Client has some team support

Description: *"Content Creator handles ideation and writing; Social Media Manager handles scheduling, analytics, and competitor monitoring. This keeps the writing pipeline separate from the publishing pipeline."*

**Platform Specialist Agents** (YouTube-primary clients)
Recommended when:
- YouTube is a serious channel with weekly long-form content
- Plus 2+ other platforms with different audiences and content styles
- Client or team has dedicated video production capacity

Description: *"YouTube Agent manages video strategy, scripting, and publishing. Social Media Manager handles all short-form platforms. These require genuinely different strategies and audiences."*

Present the recommendation with rationale. The client is free to choose differently — this is a starting point.

---

## Save Strategy to Cloud Brain

```
mcp__cloud-brain__write_note
path: brain/social-media/[business-name-slug]-strategy.md
```

Include:
- Platform tiers and reasoning
- Content pillar allocation with percentages
- Posting frequency per platform
- Optimal posting times per platform
- Content type mix
- Agent configuration recommendation
- Date created

---

## Output — Strategy Document

Present the complete strategy clearly:

---

**Social Media Strategy — [Business Name]**
*Built: [date]*

**Platform Focus:**
- Tier 1 (Primary): [platforms] — [frequency]
- Tier 2 (Secondary): [platforms] — [frequency]
- Tier 3 (Presence): [platforms] — minimal

**Content Pillars:**
| Pillar | Allocation | Purpose |
|---|---|---|
| [Pillar 1] | [%] | [role] |
| [Pillar 2] | [%] | [role] |
| [Pillar 3] | [%] | [role] |

**Posting Schedule:**
| Platform | Frequency | Best Days | Best Times |
|---|---|---|---|
| [platform] | [x/week] | [days] | [times] |

**Content Mix:** [% video] video · [% graphics] graphics · [% text] text

**Agent Configuration:** [recommendation]

**Total weekly posts:** [number]

---

*This strategy is your agent's operating document. Every content calendar, post, and schedule decision is made against it. Run sm-strategy-builder again anytime your business or goals shift.*
