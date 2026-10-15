---
name: sm-discovery
description: >-
  Social media discovery interview — learns the client's business, platforms, goals, audience, pain points, and content capacity. Saves everything to Cloud Brain to power strategy, content creation, and scheduling. Use when: 'tell me about my business', 'social media discovery', 'update my social media profile', 'redo my social media setup', or as part of sm-onboard.
---

# SM Discovery — Social Media Business & Goals Interview

## Overview

This skill conducts a structured discovery interview to build a complete picture of the client's business, social media presence, goals, audience, and content capacity. The output is saved to Cloud Brain and used by every other skill in the suite — particularly sm-strategy-builder, sm-content-creator, and sm-content-calendar.

---

## Pre-Flight — Check for Existing Discovery

Search Cloud Brain: `mcp__cloud-brain__search_notes` with query `"social media discovery"`.

**If found:** Display a summary of existing answers and ask: *"Here's what I have on file — does anything need updating, or shall we do a fresh discovery?"*

If updating: ask only the questions where answers need to change.
If fresh: proceed with the full interview below.

---

## The Discovery Interview

Conduct this as a natural conversation, not a form. Group related questions. Do not ask all questions at once — let the client answer one group before moving to the next.

---

### Group 1 — Your Business

*"Let's start with your business. Tell me about what you do and who you serve."*

Capture:
- **Business name**
- **What the business does** (product, service, or both)
- **Industry / niche** (be specific — "fitness coaching for busy moms" is better than "fitness")
- **Primary target audience** (demographics, psychographics, key pain points your product/service solves)
- **Business stage** (just starting out / established and growing / scaling)
- **Geographic focus** (local, national, international, or specific markets)

---

### Group 2 — Current Social Media Presence

*"Now let's look at where you're showing up online. Tell me about your social media accounts."*

For each platform the client mentions, capture:
- Platform name
- Handle / username / URL
- Approximate current follower count (estimate is fine)
- How active they currently are (posting daily / weekly / rarely / not at all)

**Platforms to ask about specifically if not mentioned:**
- Instagram
- Facebook (page or profile?)
- LinkedIn (personal profile or company page?)
- TikTok
- YouTube
- Pinterest
- Threads / X (Twitter) / Bluesky

Also ask: *"Are there platforms you're not on yet but are considering?"*

---

### Group 3 — Goals

*"What are you trying to achieve with social media? There's no wrong answer — I just need to understand what success looks like for you."*

Ask them to rank their goals (primary, secondary):
- **Brand awareness** — getting more people to know you exist
- **Lead generation** — attracting potential clients or customers
- **Sales / conversions** — driving direct purchases or bookings
- **Community building** — creating an engaged audience around your brand
- **Thought leadership** — establishing expertise and credibility
- **Customer retention** — staying top of mind with existing clients
- **Other** — let them describe

Also ask: *"Is there a specific outcome you want in the next 90 days from social media?"*

---

### Group 4 — Pain Points

*"What's been hardest about managing social media for your business?"*

Listen for and note:
- **Consistency** — can't stick to a posting schedule
- **Ideas** — don't know what to post
- **Time** — takes too long to create content
- **Engagement** — posting but not getting interaction or followers
- **Results** — not sure if social media is even working
- **Voice** — struggle to sound natural or professional
- **Platforms** — overwhelmed by too many platforms
- **Tech** — tools are confusing or time-consuming
- **Other** — record exactly what they say

---

### Group 5 — Content Capacity

*"This is an important one — I need to know what kind of content you can realistically produce, because it shapes everything."*

Ask:
- **Can you film video?** (short clips on your phone / professional video / both / no)
- **Do you appear on camera yourself, or prefer behind-the-scenes / product content?**
- **Can you write well, or does writing feel like a struggle?**
- **Do you have photos of your work, products, or team we can use?**
- **Do you have a podcast or record audio content?**
- **How much time per week can you realistically spend on social media content?** (less than 1 hour / 1–3 hours / 3–5 hours / more than 5 hours)

---

### Group 6 — Competitors & Inspiration

*"Who are you watching? Tell me about competitors or accounts in your space that you admire."*

Capture:
- Up to 5 competitor or industry accounts (name/handle + platform)
- What they admire about those accounts (style, consistency, content type, engagement)
- What they do NOT want to copy or emulate

---

### Group 7 — Posting Frequency & History

*"Let's talk about your current reality and your goal."*

- **Current posting frequency** (per platform, if they're active)
- **Desired posting frequency** (per platform — be realistic together)
- **Best time of day to engage** (do they know when their audience is online, or should we let Metricool data decide?)
- **Any content they've posted in the past that performed really well?** (describe it — helps with strategy)

---

## Save to Cloud Brain

After the interview, save the complete discovery profile:

```
mcp__cloud-brain__write_note
path: brain/social-media/[business-name-slug]-discovery.md
```

Also update preferences:
```
mcp__cloud-brain__write_note
path: brain/preferences/social-media-manager-preferences.md
```

Include all captured information organized by section.

---

## Output Summary

Present a clean discovery summary to the client before closing:

---

**Discovery Summary — [Business Name]**

**Business:** [what they do, who they serve, niche]
**Stage:** [business stage]
**Target Audience:** [summary]

**Active Platforms:**
| Platform | Handle | Followers | Activity |
|---|---|---|---|
| [platform] | [handle] | [count] | [frequency] |

**Primary Goal:** [goal]
**Secondary Goal:** [goal]

**Biggest Pain Point:** [pain point]

**Content Capacity:**
- Video: [yes/no/type]
- Writing: [comfortable/struggles]
- Time available: [hours/week]

**Accounts to Watch:** [competitor list]

**Desired Posting Frequency:** [per platform]

---

*This profile is saved to your AI brain and will be used to build your content strategy, write your posts, and optimize your schedule.*
