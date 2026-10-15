---
name: sm-trend-watcher
description: >-
  Researches what's trending in the client's industry and on social media — surfaces content ideas, trending formats, hashtags, and competitor activity. Use when: 'what's trending', 'trend research', 'what should I post this week', 'find trending content', 'industry trends', 'what's hot on social media', 'research trends'.
---

# SM Trend Watcher — Weekly Industry & Platform Trend Research

## Overview

This skill researches what's trending in the client's industry and on their active social media platforms each week. The output is a trend brief saved to Cloud Brain that sm-content-creator uses to sharpen this week's post topics and angles. Runs weekly — ideally on Monday morning before content creation begins.

---

## Pre-Flight — Load Client Context

1. `mcp__cloud-brain__search_notes` with query `"social media discovery"` — load industry, niche, and platforms
2. `mcp__cloud-brain__search_notes` with query `"social media strategy"` — load content pillars and platform tiers
3. `mcp__cloud-brain__search_notes` with query `"competitor"` — load competitor accounts to monitor

---

## Job Inputs (Ask Fresh Each Run)

Ask:
- **Any specific topic or trend you want me to investigate this week?** (optional — if they have a hunch or something they saw)
- **Any upcoming dates, events, or news in your industry that content should respond to?**

---

## Step 1 — Industry Trend Research

Run web searches to find what's currently resonating in the client's niche:

Search queries to use (substitute [INDUSTRY] and [NICHE] from the discovery profile):
- `"[INDUSTRY] trends [current month year]"`
- `"[NICHE] social media content ideas [current month year]"`
- `"[INDUSTRY] news this week"`
- `"what's trending on [platform] [INDUSTRY] [current month year]"`
- `"[NICHE] viral content"`

For each search, extract:
- What topics are getting the most attention right now
- What questions or conversations are happening in this space
- Any news, events, or developments that create content opportunities

---

## Step 2 — Platform Trend Research

For each Tier 1 and Tier 2 platform in the client's strategy, research platform-specific trends:

**Instagram:**
- Search: `"Instagram trends [current month year]"`, `"Instagram Reels trends [INDUSTRY]"`
- Look for: trending audio/sounds, visual formats (talking head, text overlay, transitions), challenge or format trends

**TikTok:**
- Search: `"TikTok trends [current month year] [INDUSTRY]"`, `"TikTok sounds trending"`, `"[NICHE] TikTok content"`
- Look for: trending sounds, content formats, viral content structures, hashtag trends

**LinkedIn:**
- Search: `"LinkedIn trending topics [current month year]"`, `"what's performing on LinkedIn [INDUSTRY]"`
- Look for: content formats (text posts vs. carousels vs. video), topic themes, thought leadership angles

**YouTube:**
- Search: `"trending YouTube topics [INDUSTRY] [current month year]"`, `"[NICHE] YouTube Shorts trends"`
- Look for: search intent (what are people looking for?), trending formats, high-performing topic angles

**Facebook:**
- Search: `"Facebook content trends [current month year]"`, `"[INDUSTRY] Facebook groups trending topics"`
- Look for: group conversations, share-worthy content formats

---

## Step 3 — Hashtag Research

For the client's primary platforms (Instagram and TikTok especially):

Search for:
- Top hashtags in their niche (high-volume but not oversaturated)
- Micro-hashtags in their specific niche (10K–500K posts — better reach for smaller accounts)
- Any hashtags currently trending in their industry

Produce a refreshed hashtag set for this week:
- 5 broad hashtags (industry-wide, 1M+ posts)
- 5 mid-range hashtags (niche-specific, 100K–1M posts)
- 5 micro hashtags (highly targeted, 10K–100K posts)

---

## Step 4 — Competitor Activity Snapshot

Using web search (and Metricool competitor data if connected):

For each competitor account saved in the client's discovery profile:
- What did they post this week that got strong engagement?
- Are they jumping on any trend the client hasn't addressed?
- Any content format they're using that's getting traction?

Note: this is for awareness and inspiration, not copying. The goal is to identify gaps — content opportunities the competitors are missing that the client can own.

---

## Step 5 — Content Ideas

Based on all research, generate 5–7 specific content ideas for this week:

For each idea, provide:
- **Platform** (where this works best)
- **Pillar** (which content pillar it falls under)
- **Format** (video, graphic, text, carousel)
- **Hook / angle** (one-line version of the idea, including a compelling hook)
- **Why now** (what trend or context makes this timely)

Present the ideas ranked by estimated impact / current relevance.

---

## Save Trend Brief to Cloud Brain

```
mcp__cloud-brain__write_note
path: brain/social-media/[business-name-slug]-trends-[YYYY-MM-DD].md
```

---

## Output — Weekly Trend Brief

---

**📡 Weekly Trend Brief — [Business Name] — Week of [date]**

---

**🔥 What's Hot in [Industry/Niche] Right Now**

[2–3 sentences on the most relevant industry trend or news this week]

---

**📱 Platform Trends**

**Instagram:** [trending format or sound + brief description]
**TikTok:** [trending sound or format + brief description]
**LinkedIn:** [trending topic or format]
**YouTube:** [trending search topic or format]
*(Include only active platforms)*

---

**#️⃣ Hashtag Set for This Week**

Broad: #[tag] #[tag] #[tag] #[tag] #[tag]
Mid: #[tag] #[tag] #[tag] #[tag] #[tag]
Micro: #[tag] #[tag] #[tag] #[tag] #[tag]

---

**👀 Competitor Moves**

- [Competitor] posted [format] about [topic] — got [engagement signal]
- [Content gap identified]: Nobody in this niche is talking about [topic] — opportunity

---

**💡 Content Ideas for This Week**

1. **[Hook / angle]** · [Platform] · [Pillar] · [Format]
   *Why now: [trend context]*

2. **[Hook / angle]** · [Platform] · [Pillar] · [Format]
   *Why now: [trend context]*

3. **[Hook / angle]** · [Platform] · [Pillar] · [Format]
   *Why now: [trend context]*

4. **[Hook / angle]** · [Platform] · [Pillar] · [Format]
   *Why now: [trend context]*

5. **[Hook / angle]** · [Platform] · [Pillar] · [Format]
   *Why now: [trend context]*

---

*Run sm-content-creator next — it will use this trend brief to sharpen your posts.*
