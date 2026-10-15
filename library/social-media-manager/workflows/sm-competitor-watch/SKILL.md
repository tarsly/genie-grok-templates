---
name: sm-competitor-watch
description: >-
  Monitors competitor and industry accounts — surfaces what content is getting traction, identifies gaps and opportunities, and flags trends to capitalize on. Use when: 'watch my competitors', 'competitor analysis', 'what are my competitors posting', 'industry account monitor', 'what's working for others in my space', 'spy on competitors'.
---

# SM Competitor Watch — Industry & Competitor Monitoring

## Overview

This skill monitors competitor and industry accounts to identify what content is getting the most traction, what topics and formats are resonating in the client's space, and where content gaps exist that the client can move into. It uses Metricool's competitor data when available and supplements with web research.

Can run weekly alongside sm-trend-watcher, or on-demand when the client wants a deeper competitive read.

---

## Pre-Flight — Load Context

1. `mcp__cloud-brain__search_notes` with query `"social media discovery"` — load competitor list and industry
2. `mcp__cloud-brain__search_notes` with query `"social media strategy"` — load content pillars and platforms

---

## Job Inputs (Ask Fresh Each Run)

Ask:
- **Do you want to monitor all saved competitors, or focus on specific ones this week?**
- **Any specific platform to focus on?** (or "all active platforms")
- **Any new competitors or accounts to add to your watch list?** (update Cloud Brain if yes)

---

## Step 1 — Metricool Competitor Data

If Metricool MCP is connected, retrieve competitor data:
- `get_network_competitors` — list configured competitor accounts
- `get_network_competitors_posts` — recent posts and their performance metrics

For each competitor, extract:
- Their top-performing post this week / month (highest engagement)
- What platform it was on
- What format (video, graphic, text, carousel)
- What topic
- Estimated engagement rate

**If competitors are not yet set up in Metricool:** Prompt: *"Add your competitors to Metricool by going to Metricool → Analytics → Competitors → Add. Once they're tracked, I'll pull their actual metrics. For now, I'll research them via web search."*

---

## Step 2 — Web Research on Competitors

For each competitor account saved in the client's profile:

Search: `"[competitor name] [platform] recent posts"`, visit their public profiles where possible, and look for:
- Most recent content — what topics and formats are they using?
- Any posts with visible high engagement (likes, comments)
- Content cadence — how often are they posting?
- Any recent campaigns or launches

Also search: `"[industry] top accounts [platform] [current year]"` to identify any influential industry accounts the client should be aware of beyond their direct competitors.

---

## Step 3 — Gap Analysis

Compare what competitors are doing to the client's current content strategy:

**Content they're doing that the client isn't:**
- Topics: Are competitors covering topics in the client's pillars that the client hasn't posted about?
- Formats: Are competitors using video while the client is only doing graphics?
- Posting frequency: Are competitors posting more often and getting more visibility?

**Content the client is doing that competitors aren't:**
- These are potential differentiators — note them and recommend doubling down

**Topics nobody in the space is covering well:**
- This is the highest-value insight — a content gap the client can own

---

## Step 4 — Opportunity Summary

From the analysis, produce 3–5 specific opportunities:

Format each as:
- **The opportunity**
- **Evidence** (what you saw that points to this)
- **How to act on it** (specific content idea or strategy shift)

---

## Save to Cloud Brain

```
mcp__cloud-brain__write_note
path: brain/social-media/[business-name-slug]-competitor-watch-[YYYY-MM-DD].md
```

---

## Output — Competitor Watch Report

---

**👀 Competitor Watch — [Business Name] — [date]**

---

**Accounts Monitored:** [list of competitors]
**Platforms:** [list]

---

**🏆 What's Getting Traction in Your Space**

| Account | Post Topic | Platform | Format | Why It Worked |
|---|---|---|---|---|
| [competitor] | [topic] | [platform] | [format] | [reason] |
| [competitor] | [topic] | [platform] | [format] | [reason] |
| [competitor] | [topic] | [platform] | [format] | [reason] |

---

**📋 What Competitors Are Doing That You're Not**

[Specific notes — 2–4 observations with content implications]

---

**💪 What You're Doing That They're Not**

[Your differentiators — reinforce these]

---

**🎯 Content Gaps Nobody Is Owning**

[Topics or formats that are underserved in the space — first-mover opportunities]

---

**💡 Top Opportunities This Week**

1. **[Opportunity]**
   *Evidence: [what you saw] → Action: [specific content recommendation]*

2. **[Opportunity]**
   *Evidence: [what you saw] → Action: [specific content recommendation]*

3. **[Opportunity]**
   *Evidence: [what you saw] → Action: [specific content recommendation]*

---

*Pass these insights to sm-trend-watcher or sm-content-creator to apply them this week.*
