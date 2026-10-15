---
name: sm-repurpose
description: >-
  Repurposes existing content into platform-optimized social media posts — turns blog posts, video transcripts, podcast episodes, newsletters, or any long-form content into a week's worth of posts across all platforms. Use when: 'repurpose this content', 'turn this into posts', 'I have a blog post to repurpose', 'make social posts from this', 'repurpose my podcast', 'create posts from this article', 'repurpose my newsletter'.
---

# SM Repurpose — Content Repurposing Engine

## Overview

This skill takes any piece of long-form or existing content — a blog post, video transcript, podcast episode, newsletter, webinar recording, or even a great email — and transforms it into a full set of platform-optimized social media posts. One piece of content becomes 5–10 posts across all active platforms. Output is saved to Cloud Brain and flows into sm-schedule-queue for scheduling.

---

## Pre-Flight — Load Client Context

1. `mcp__cloud-brain__search_notes` with query `"brand snapshot"` — load brand voice and content pillars
2. `mcp__cloud-brain__search_notes` with query `"social media strategy"` — load active platforms and posting style
3. `mcp__cloud-brain__search_notes` with query `"social media discovery"` — load audience and tone

---

## Job Inputs (Ask Fresh Each Run)

Ask:
- **What content are you repurposing?** (blog post, podcast episode, video, newsletter, webinar, email, other)
- **Please share the content** — paste the text, or tell me the title/topic if I should search for it
- **Is this content publicly available?** (if yes and they share a URL, fetch it)
- **Any specific angle or audience segment you want to target with this repurposed content?**
- **Which platforms do you want posts for?** (or "all active platforms")
- **Are there any quotes, stats, or moments from this content that are especially strong?** (optional — helps prioritize)

Do not save these answers. All are job inputs.

---

## Step 1 — Analyze the Source Content

Read the content the client provided and extract:

- **Core message** — what is the single most important point?
- **Key supporting points** — 3–7 sub-ideas or supporting arguments
- **Quotable moments** — short, punchy phrases that stand alone
- **Statistics or data points** — any numbers worth highlighting
- **Stories or examples** — relatable moments or case studies
- **Actionable takeaways** — what should the reader/viewer do with this?
- **Questions raised** — what questions does this content answer?

---

## Step 2 — Map Content to Posts

Plan out the posts to be created. The goal is multiple angles from one piece of content — not the same post slightly reworded.

**Content repurposing matrix:**

| Source | Platform | Format | Angle |
|---|---|---|---|
| Core message | LinkedIn | Long-form text post | Full argument |
| Core message | Instagram | Carousel | 5-slide breakdown |
| Key stat #1 | Instagram | Graphic | Data point post |
| Quotable moment | All | Graphic + caption | Pull quote |
| Supporting point #1 | TikTok | Script | "Did you know?" video |
| Supporting point #2 | Facebook | Text post | Conversational angle |
| Action takeaway | All | Text | CTA-focused post |
| Story/example | Instagram/LinkedIn | Text | Personal story angle |

Adapt the matrix to the client's active platforms and content capacity.

---

## Step 3 — Write All Posts

Write each post using the client's brand voice. Apply the same platform-specific rules as sm-content-creator:

**Instagram:** Hook + body + CTA + hashtags (5–15). Include visual brief.

**TikTok:** Hook (first 3 seconds) + full script + caption. Include visual/format brief.

**LinkedIn:** Scroll-stopping opener + well-spaced body paragraphs + question or CTA. 150–600 words. 3–5 hashtags.

**Facebook:** Conversational opener + body + engagement question.

**YouTube:** If source is a video or podcast: title + full description + tags + thumbnail concept.

For each post, also write a **visual brief** so sm-visual-creator can create the graphic.

**Tone note:** Each post should feel like an original piece of content, not a summary. Draw out the strongest angle for each platform's audience.

---

## Step 4 — Content Upload Reminder (for Video-Based Repurposing)

If the source content is a video or podcast and the client needs to create short-form clips:

Remind them of the upload workflow:

---

*To turn this content into a short-form video post:*

1. Identify the strongest 30–60 second moment from your video/episode
2. Clip it using [their video tool — CapCut, Descript, etc.]:
   - CapCut: Import → trim to clip → add captions → export vertical (1080×1920) as MP4
   - Descript: Open project → select clip → export as MP4, 1080×1920 for Reels/TikTok
3. Upload the clip to Metricool: Planner → New Post → attach video → set the caption I've drafted below
4. Tell me: *"Video clip for [topic] is uploaded to Metricool"* and I'll link it to the scheduled post

*Alternatively, I can create a text-overlay version of the key quote for Instagram/TikTok — no video filming required. Just confirm.*

---

## Step 5 — Save to Cloud Brain

Save all repurposed posts as a draft batch:

```
mcp__cloud-brain__write_note
path: brain/social-media/[business-name-slug]-repurposed-[YYYY-MM-DD].md
```

Mark each post:
- Source: `REPURPOSED from [source title/type]`
- Platform
- Pillar (which content pillar it falls under)
- Approval status: `PENDING`

---

## Output — Repurposed Content Pack

---

**🔄 Repurposed Content — [Business Name]**
*Source: [title or type of original content]*
*[n] posts created across [n] platforms*

---

**INSTAGRAM — Carousel — [Core message]**
*Pillar: [pillar] | Visual: carousel required*

**Slide 1 (Cover):** [headline]
**Slide 2:** [point 1]
**Slide 3:** [point 2]
**Slide 4:** [point 3]
**Slide 5:** [point 4]
**Slide 6 (CTA):** [call to action]

Caption: [hook + brief description + CTA]
Hashtags: [hashtags]

---

**INSTAGRAM — Graphic — [Pull quote]**
*Pillar: [pillar]*

[Quote]

[Caption]

Visual brief: [brief]

---

**LINKEDIN — Text Post — [Core argument]**
*Pillar: [pillar]*

[Full LinkedIn post]

---

**TIKTOK — Script — ["Did you know?" angle]**
*Pillar: [pillar]*

Hook (spoken): [hook]
[Full script]
Caption: [caption]

---

**FACEBOOK — Post — [Conversational angle]**
*Pillar: [pillar]*

[Post]

---

*(Continue for all platforms)*

---

**Total posts created:** [n]
**Next steps:** Run sm-visual-creator for graphics → sm-schedule-queue to approve and schedule
