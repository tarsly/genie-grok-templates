---
name: sm-content-creator
description: >-
  Writes this week's social media posts — platform-optimized captions, hooks, CTAs, hashtags, and visual briefs for Instagram, TikTok, LinkedIn, Facebook, and YouTube. Use when: 'write my posts', 'create this week's content', 'write social media captions', 'draft my posts', 'content for this week', 'write posts for [platform]'.
---

# SM Content Creator — Weekly Post Writing

## Overview

This skill writes a full week of platform-optimized social media posts using the client's brand voice, content pillars, strategy, and calendar from Cloud Brain. Each post includes a caption, hook, CTA, hashtags (where applicable), and a visual brief for sm-visual-creator. Output is saved to Cloud Brain for approval in sm-schedule-queue.

---

## Pre-Flight — Load Client Context

Load all of the following from Cloud Brain before writing a single word:

1. `search_notes` query `"social media strategy"` — platform tiers, posting frequency, timing
2. `search_notes` query `"brand snapshot"` or `"brand audit"` — voice, pillars, visual style
3. `search_notes` query `"social media discovery"` — business, audience, tone
4. `search_notes` query `"content calendar"` — current month's calendar with this week's topics
5. `search_notes` query `"trend brief"` — trend-watcher output (if run this week)

**If brand data is missing:** Do not write. *"I need your brand profile before creating content. Run sm-brand-snapshot first."*

**If strategy is missing:** Do not write. *"I need your strategy before creating content. Run sm-strategy-builder first."*

---

## Job Inputs (Ask Fresh Each Run)

Ask:
- **Which week is this for?** (e.g., "week of July 7")
- **Any specific topics, announcements, or products to feature this week?**
- **Any constraints — topics to avoid, tone shifts (more serious, lighter), length preferences?**
- **Are there any trending audio or sounds you want to use on TikTok/Reels this week?** (optional)

Do not save these answers to Cloud Brain.

---

## Step 1 — Identify This Week's Posts

Reference the saved content calendar for this week's planned posts. If no calendar exists, default to the strategy's posting frequency and pillar allocation.

List out the posts to be written:
- Platform, pillar, content type, and topic angle for each post this week

Confirm with the client before writing: *"Here's this week's post plan — [list]. Any changes before I start writing?"*

---

## Step 2 — Write Posts by Platform

Write every post using the client's brand voice. Apply these platform-specific rules:

---

### Instagram

**Caption structure:**
1. **Hook** (first line — must stop the scroll. No "Hey guys!" or weak openers. Use a question, bold statement, contrarian take, or number.)
2. **Body** (2–4 short paragraphs, line breaks between each, conversational tone)
3. **CTA** (one clear action: comment, save, share, click link in bio, DM)
4. **Hashtags** (5–15 relevant hashtags; mix of niche, medium, and broad; placed after the caption or in first comment)

**Length:** 150–300 words for feed posts. Stories are separate (see below).

**Visual brief:** Specify image dimensions (1080×1080 for square, 1080×1350 for portrait), background style, text overlay if any, brand colors to use.

---

### TikTok

**Script structure:**
1. **Hook** (first 3 seconds — spoken hook that interrupts the scroll: "POV:", "Stop scrolling if...", "Here's what nobody tells you about...")
2. **Body** (script for 30–90 second video — short sentences, one idea per sentence)
3. **CTA** (comment, follow, visit link in bio)
4. **Caption** (short, 50–100 words — TikTok captions are secondary to video)
5. **Hashtags** (3–5 relevant, including #[industry] and trending tags if applicable)

**Visual brief:** Specify if this is talking-head, voiceover + text, product demo, or B-roll. Aspect ratio: 1080×1920.

---

### LinkedIn

**Post structure:**
1. **Hook** (first 1–2 lines — bold statement or insight. Must make someone stop before "see more")
2. **Body** (2–5 short paragraphs; one idea per paragraph; line breaks essential; lists only when needed)
3. **CTA** (question to the audience, invitation to share perspective, or link to resource)

**Tone:** Professional but personal. First-person. Opinions welcome. No corporate speak.

**Length:** 150–600 words. Longer posts often outperform on LinkedIn.

**Hashtags:** 3–5 maximum, placed at the end. LinkedIn hashtags are category signals, not discovery tools.

**Visual brief:** If using an image — 1200×627. Text-only posts also perform well on LinkedIn; don't force an image.

---

### Facebook

**Post structure:**
1. **Opening** (conversational — can be softer than Instagram; think "talking to a friend")
2. **Body** (2–3 paragraphs; shorter than LinkedIn; include a question to drive comments)
3. **CTA** (comment, share, tag someone, visit link)

**Length:** 100–250 words. Facebook rewards native posts over link posts.

**Hashtags:** Optional. 1–2 maximum. Facebook hashtags have minimal discovery value.

**Visual brief:** 1200×630 for feed images.

---

### YouTube

**For Shorts (< 60 seconds):**
- Same as TikTok script — hook, body, CTA
- Include thumbnail concept (bold text, expressive face or striking image, high contrast)
- Description: 100–200 words with keywords

**For Long-form videos:**
- Title (60 characters max, includes primary keyword, is click-worthy but not clickbait)
- Description (300–500 words: first 2 lines must include keywords and hook; timestamps if applicable; links to resources; CTA to subscribe)
- Tags (10–15 keyword tags)
- Thumbnail concept (text overlay + image description)
- Chapter markers (if video > 8 minutes)

---

## Step 3 — Visual Briefs

For every post requiring a graphic or designed visual, write a visual brief:

```
VISUAL BRIEF — [Platform] — [Topic]
Dimensions: [W × H]
Background: [color/style]
Main element: [image or icon description]
Text overlay: [exact text, if any]
Brand colors: [primary/secondary]
Style: [clean/bold/warm/etc.]
Reference: [any inspiration or examples]
```

These briefs are passed to sm-visual-creator automatically.

---

## Save Drafts to Cloud Brain

Save all posts for the week as a single note:

```
mcp__cloud-brain__write_note
path: brain/social-media/[business-name-slug]-drafts-[YYYY-MM-DD].md
```

Mark each post with:
- Platform
- Pillar
- Content type
- Publish date/time (from strategy)
- Approval status: `PENDING`

---

## Output Format

Present all posts cleanly, one platform section at a time:

---

**Weekly Content Drafts — [Business Name] — Week of [date]**

---

**INSTAGRAM — [date] — [Pillar]**
*Type: [graphic/video/carousel] | Publish: [day, time]*

**Hook:** [first line]

[Full caption]

**CTA:** [CTA]

**Hashtags:** #[tag] #[tag] #[tag]

**Visual Brief:**
[brief]

---

*(Repeat for each post across all platforms)*

---

**This Week's Posts: [count] | Ready for: sm-visual-creator → sm-schedule-queue**

*Tip: Run sm-visual-creator next to create the graphics, then sm-schedule-queue to review and publish.*
