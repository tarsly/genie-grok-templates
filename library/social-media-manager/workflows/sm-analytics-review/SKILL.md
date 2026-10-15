---
name: sm-analytics-review
description: >-
  Monthly social media performance review — pulls analytics from Metricool, surfaces top content, identifies trends, and recommends strategy adjustments. Use when: 'review my analytics', 'how did my posts perform', 'social media report', 'monthly analytics', 'what's working', 'performance review', 'check my social media stats'.
---

# SM Analytics Review — Monthly Performance Report

## Overview

This skill pulls performance data from Metricool, analyzes what's working, identifies patterns in top-performing content, and produces recommendations for the next month's strategy. It runs monthly and saves insights to Cloud Brain to inform sm-content-calendar and sm-strategy-builder.

---

## Pre-Flight — Load Context

1. `mcp__cloud-brain__search_notes` with query `"social media strategy"` — load strategy to compare against
2. `mcp__cloud-brain__search_notes` with query `"approval history"` — cross-reference with performance
3. `mcp__cloud-brain__search_notes` with query `"social media discovery"` — confirm active platforms and goals

---

## Job Inputs (Ask Fresh Each Run)

Ask:
- **Which month are we reviewing?** (e.g., "June 2026")
- **Any specific platform you want to focus on?** (or "all of them")
- **Any specific question you want answered from the data?** (e.g., "why did my LinkedIn engagement drop in week 3?")

---

## Step 1 — Pull Metricool Data

Using the Metricool MCP, retrieve data for the review period.

For each active platform, call the appropriate Metricool tool:
- Instagram: `get_instagram_posts`, `get_instagram_reels`, `get_instagram_stories`
- TikTok: `get_tiktok_videos`
- Facebook: `get_facebook_posts`, `get_facebook_reels`
- LinkedIn: `get_linkedin_posts`
- YouTube: `get_youtube_videos`
- Pinterest: `get_pinterest_pins`
- X/Twitter: `get_x_posts`

Also call:
- `get_analytics` — overall account metrics
- `get_network_competitors` — competitor context for benchmarking
- `get_best_time_to_post` — check if optimal times have shifted

Filter all calls to the review month's date range.

**If Metricool is not connected:** Ask the client to share their analytics manually — request screenshots or a summary of their top posts, reach, engagement, and follower growth for the month. Proceed with what they provide and note: *"Connect Metricool (https://ai.metricool.com/mcp) to automate this data pull next month."*

---

## Step 2 — Top Content Analysis

From the data retrieved, identify:

**Top 3 posts by engagement rate** (likes + comments + shares / reach × 100):
- What platform
- What pillar
- What content type (video, graphic, text, carousel)
- What the hook or opening was
- Why it likely performed well (note patterns)

**Bottom 3 posts by engagement rate:**
- Same fields
- Why it likely underperformed

**Best performing content type** (e.g., Reels outperformed graphics 2:1)

**Best performing content pillar** (e.g., "Client Success" posts averaged 4.2% engagement vs. 1.8% for "Promotional")

---

## Step 3 — Platform Performance Summary

For each active platform, calculate:
- **Total posts published**
- **Average reach per post**
- **Average engagement rate**
- **Follower growth** (net new followers this month)
- **Best posting day and time** (based on actual performance this month)
- **Compared to strategy:** On track / underperforming / outperforming

---

## Step 4 — Goal Progress Check

Reference the client's goals from the discovery profile. For each primary goal:

**Brand Awareness:** Did reach / impressions grow month-over-month?
**Lead Generation:** Did profile visits and link-in-bio clicks increase?
**Sales:** Did any posts directly reference an offer? What was the response?
**Community Building:** Did comment volume and follower growth trend up?
**Thought Leadership:** Were any posts saved or shared at high rates?

Use available data to estimate progress. Note where data is unavailable.

---

## Step 5 — Competitor Benchmarking

If Metricool competitor data is available (`get_network_competitors_posts`):
- Compare the client's average engagement rate against the top 3 competitors tracked
- Note any content types competitors used this month that the client hasn't tried
- Identify any competitor content that went unusually viral — note the topic and format

---

## Step 6 — Recommendations

Based on all data, produce 3–5 specific, actionable recommendations for next month:

Format each recommendation as:
- **What to do**
- **Why** (data that supports it)
- **How to apply** (specific action in next month's calendar or strategy)

Examples:
- *"Double down on Reels — they averaged 3.8% engagement vs. 1.2% for static posts. Replace 2 static posts/week with short Reels using the same educational content."*
- *"Post LinkedIn on Tuesday and Thursday only — Wednesday and Friday posts averaged 40% less reach."*
- *"Your 'Client Success' pillar is outperforming every other pillar. Add a 4th weekly post specifically for client wins or testimonials."*

---

## Save Insights to Cloud Brain

```
mcp__cloud-brain__write_note
path: brain/social-media/[business-name-slug]-analytics-[YYYY-MM].md
```

Include: raw summary stats, top/bottom posts, recommendations, and date of review.

---

## Output — Monthly Analytics Report

---

**Social Media Performance Report — [Business Name] — [Month Year]**
*Pulled from Metricool · Reviewed: [date]*

---

**📊 At a Glance**

| Platform | Posts | Avg Reach | Avg Engagement | Follower Growth |
|---|---|---|---|---|
| Instagram | [n] | [n] | [%] | +[n] |
| LinkedIn | [n] | [n] | [%] | +[n] |
| TikTok | [n] | [n] | [%] | +[n] |
| Facebook | [n] | [n] | [%] | +[n] |

---

**🏆 Top Posts This Month**

1. **[Post topic]** — [platform] — [engagement rate]%
   *Why it worked: [reason]*

2. **[Post topic]** — [platform] — [engagement rate]%
   *Why it worked: [reason]*

3. **[Post topic]** — [platform] — [engagement rate]%
   *Why it worked: [reason]*

---

**📉 What Underperformed**

[Brief notes on low performers — no judgment, just patterns]

---

**📈 What's Working**

- Best content type: [type]
- Best content pillar: [pillar]
- Best platform: [platform]
- Best posting day/time: [day/time]

---

**🎯 Goal Progress**

| Goal | Status | Notes |
|---|---|---|
| [Goal] | [On track / Lagging / Ahead] | [data point] |

---

**📋 Recommendations for Next Month**

1. [Recommendation with data reasoning]
2. [Recommendation with data reasoning]
3. [Recommendation with data reasoning]

---

*Run sm-content-calendar next to apply these recommendations to next month's plan.*
