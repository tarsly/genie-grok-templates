---
name: sm-brand-snapshot
description: >-
  Brand voice and identity capture for social media — defines brand voice, visual identity, and content pillars. Checks for existing brand audit before asking questions. Use when: 'define my brand', 'capture my brand voice', 'set up brand for social media', 'brand snapshot', 'update my brand', or as part of sm-onboard.
---

# SM Brand Snapshot — Brand Voice & Identity for Social Media

## Overview

This skill captures the brand identity that every piece of content will be written to match. It checks for an existing brand profile first — from the brand-toolkit plugin or a prior snapshot. If nothing exists, it offers a full brand audit or a quick snapshot. The result is saved to Cloud Brain and referenced by sm-content-creator, sm-visual-creator, and sm-strategy-builder.

---

## Pre-Flight — Check for Existing Brand Data

Search Cloud Brain in this order:

1. `mcp__cloud-brain__search_notes` with query `"brand audit"` — checks for a full brand-toolkit profile
2. `mcp__cloud-brain__search_notes` with query `"brand snapshot"` — checks for a prior sm-brand-snapshot
3. `mcp__cloud-brain__search_notes` with query `"brand preferences"` — catches any other brand data

**If a full brand audit is found:** Display the key findings (brand voice, pillars, visual identity) and ask: *"I found your brand profile from [date]. Does this still reflect your brand, or has anything changed?"*

If current: save a confirmation timestamp and proceed to output. No re-interview needed.
If outdated: ask which sections need updating and collect only those.

**If a quick snapshot is found (but no full audit):** Confirm the snapshot and offer to expand: *"I have a basic brand snapshot on file. Would you like to run a full brand audit for a deeper analysis, or is the snapshot sufficient?"*

**If nothing is found:** Offer a choice (see below).

---

## Brand Audit vs. Quick Snapshot

If no brand data exists, present this choice:

*"Before we build your content strategy, I need to understand your brand — how you sound, what you stand for, and what makes you distinctive.*

*You have two options:*

*1. **Full Brand Audit** (~20 minutes) — A comprehensive analysis of your online presence, competitor positioning, content gaps, and a complete brand identity profile. Best if you haven't formally defined your brand before. Run the **MKT-brand-audit** skill for this.*

*2. **Quick Brand Snapshot** (~10 minutes) — The essential brand questions needed to write consistent content. Covers voice, pillars, and visual guidelines. We can run a full audit later.*

*Which would you prefer?"*

If they choose **Full Brand Audit**: Tell them to run the MKT-brand-audit skill now, and that sm-brand-snapshot will automatically read the results when they come back.

If they choose **Quick Brand Snapshot**: Proceed with the interview below.

---

## Quick Brand Snapshot Interview

---

### Brand Voice

*"Let's define how your brand sounds. When someone reads your content, how should they feel?"*

Ask:
- **Choose 3 words that describe your brand voice** (examples: warm, authoritative, playful, bold, empathetic, conversational, professional, witty, inspiring, educational, direct)
- **What voice do you want to AVOID?** (examples: corporate, preachy, salesy, formal, aggressive)
- **Provide a "voice example"** — share a piece of content you've written or a brand you admire that sounds like you. If they can't think of one, that's fine.

---

### Brand Story & Differentiators

*"What makes you different from everyone else in your space?"*

Ask:
- **Your unique value proposition** — what do clients get from you that they can't get elsewhere?
- **Your origin story or 'why'** — why do you do what you do? (This becomes content gold)
- **Your biggest client transformation** — what does life look like for a client before vs. after working with you?

---

### Content Pillars

*"Content pillars are the 3–5 themes you post about consistently. They should connect your expertise, your audience's interests, and your business goals."*

Ask: *"What topics do you know deeply and could talk about for hours?"*

Then help them define 3–5 content pillars. Each pillar should be:
- Relevant to their audience
- Connected to their business
- Something they can create content about consistently

Examples of content pillar sets:
- Life coach: Mindset | Productivity | Relationships | Career | Personal Stories
- Real estate agent: Market Updates | Home Buying Tips | Seller Strategies | Community | Behind the Scenes
- Fitness coach: Workouts | Nutrition | Mindset | Client Wins | Lifestyle

Work with the client to name and define their specific pillars. Aim for 3–5.

---

### Visual Identity

*"Now let's capture your visual style so graphics look consistent."*

Ask:
- **Brand colors** — primary and secondary (hex codes if they know them, or descriptions like "navy blue and gold")
- **Fonts** — do you use specific fonts in your brand? (optional — Canva will default to brand fonts if set up)
- **Visual style** — which best describes your aesthetic? (clean and minimal / bold and colorful / warm and earthy / professional and corporate / fun and playful / editorial / other)
- **Do you have a logo?** — yes/no. If yes, is it uploaded to Canva already?
- **Photo style** — are your photos lifestyle/candid, polished/professional, product-focused, or a mix?

---

### Posting Persona (Optional but Valuable)

*"One last question — who is the voice behind your content? This helps me write posts that sound like a real person, not a brand."*

Ask:
- **Do you post as yourself (personal brand) or as the business?**
- **Do you share personal stories, opinions, and behind-the-scenes moments?** Or do you keep it strictly professional?
- **Any topics that are off-limits** — things you never want posted about your business or your personal life?

---

## Save to Cloud Brain

Save the complete brand profile:

```
mcp__cloud-brain__write_note
path: brain/social-media/[business-name-slug]-brand.md
```

Also note in preferences:
```
mcp__cloud-brain__write_note
path: brain/preferences/social-media-manager-preferences.md
```

---

## Output Summary

Present the brand profile clearly:

---

**Brand Profile — [Business Name]**

**Voice:** [3 adjectives] — [one sentence description of the voice]
**Avoid:** [voice to avoid]

**Content Pillars:**
1. [Pillar Name] — [brief description]
2. [Pillar Name] — [brief description]
3. [Pillar Name] — [brief description]
4. [Pillar Name] — [brief description] *(if applicable)*
5. [Pillar Name] — [brief description] *(if applicable)*

**Visual Style:** [style descriptor]
**Colors:** [primary] / [secondary]
**Photo Style:** [style]

**Posting Persona:** [personal brand / business brand / blend — description]
**Off-limits:** [any stated restrictions]

---

*This brand profile is saved to your AI brain. Every post your agent creates will be written to match this voice and these pillars.*
