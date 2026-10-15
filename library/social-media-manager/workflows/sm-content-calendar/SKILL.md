---
name: sm-content-calendar
description: >-
  Generates a monthly social media content calendar — maps content pillars to posting schedule, assigns content types, and themes each week. Use when: 'create a content calendar', 'plan next month's content', 'what should I post this month', 'monthly content plan', 'content calendar', 'plan my posts'.
---

# SM Content Calendar — Monthly Content Planning

## Overview

This skill generates a monthly content calendar based on the client's saved strategy. It maps content pillars to specific posting days, assigns content types, and sets weekly themes to give the content creator clear direction. The calendar is saved to Cloud Brain and referenced by sm-content-creator each week.

---

## Pre-Flight — Load Strategy

`mcp__cloud-brain__search_notes` with query `"social media strategy"`

**If strategy not found:** Do not proceed. *"I need your social media strategy before building a calendar. Please run sm-strategy-builder first."*

**If strategy found:** Load platform tiers, content pillars, posting frequency, and timing.

---

## Job Inputs (Ask Fresh Each Run)

Ask:
- **Which month is this calendar for?** (e.g., July 2026)
- **Any upcoming events, launches, promotions, or important dates this month?** (product launch, sale, holiday, local event, speaking engagement, etc.)
- **Any themes or campaigns you want to run this month?**
- **Anything you want to avoid posting about this month?**

Do not save these answers — they are specific to this calendar run.

---

## Step 1 — Map the Month

Using the month provided, lay out:
- Total days in the month
- Which days fall on weekends
- Any holidays or notable dates relevant to their industry
- Events and promotions the client mentioned

---

## Step 2 — Assign Weekly Themes

Divide the month into 4 (or 5) weekly themes. Themes make content creation easier because every post has a directional anchor.

**Theme examples by industry:**

*Service businesses:* Week 1: Client Education | Week 2: Behind the Scenes | Week 3: Client Results | Week 4: Community/Inspiration

*E-commerce:* Week 1: Product Features | Week 2: Customer Stories | Week 3: How-To/Education | Week 4: Lifestyle/Brand

*Professional services:* Week 1: Industry Insights | Week 2: Common Mistakes | Week 3: Client Success | Week 4: Personal Story

Adapt themes to the client's content pillars from their strategy. Add a campaign week if they have a launch or promotion.

---

## Step 3 — Build the Calendar Grid

For each platform in Tier 1 and Tier 2 of their strategy, create a posting schedule for the month.

**Calendar format per platform:**

| Week | Mon | Tue | Wed | Thu | Fri | Sat | Sun |
|---|---|---|---|---|---|---|---|
| Wk 1 | [post] | — | [post] | — | [post] | — | — |
| Wk 2 | [post] | — | [post] | — | [post] | — | — |

Each cell should include:
- **Pillar** (abbreviated — e.g., "EDU" for educational, "PROMO" for promotional)
- **Content type** (graphic, video, text, carousel, story)
- **Topic angle** (one-line topic idea — e.g., "3 mistakes to avoid when hiring a contractor")

---

## Step 4 — Promotional Content Placement

If the client has any launches or promotions this month:
- Build a pre-launch runway (3–5 posts building awareness before the launch date)
- Schedule the launch announcement post
- Plan 2–3 follow-up posts (social proof, urgency, last chance)

Promotional content should not exceed 20% of the month's total posts unless the entire month is a launch campaign.

---

## Step 5 — Stories & Ephemeral Content

For Instagram and Facebook Stories (if used):
- Schedule 3–5 story slots per week
- Stories should be lighter — polls, Q&As, behind-the-scenes, quick tips, or reminders about feed posts
- These are separate from the main feed calendar

---

## Save Calendar to Cloud Brain

```
mcp__cloud-brain__write_note
path: brain/social-media/[business-name-slug]-calendar-[YYYY-MM].md
```

---

## Output — Monthly Calendar

Present the full calendar clearly. Include a platform-by-platform weekly breakdown.

---

**Content Calendar — [Business Name] — [Month Year]**

**Monthly Theme Arc:** [brief description of the month's narrative]

**Upcoming Events/Promotions:** [list]

---

**[Platform Name] — [frequency/week]**

| Week | Date | Pillar | Type | Topic |
|---|---|---|---|---|
| Week 1 | [date] | Educational | Graphic | [topic idea] |
| Week 1 | [date] | Personal | Text | [topic idea] |
| Week 2 | [date] | Client Results | Carousel | [topic idea] |
| ... | ... | ... | ... | ... |

*(Repeat for each active platform)*

---

**Story Plan:**
- Week 1: [theme] — poll or Q&A
- Week 2: [theme] — behind-the-scenes
- Week 3: [theme] — reminder + engagement
- Week 4: [theme] — wrap-up or teaser

---

**Total Posts This Month:** [count by platform]

*Tip: Run sm-trend-watcher each Monday to sharpen the specific topic angle for that week's posts before sm-content-creator writes them.*
