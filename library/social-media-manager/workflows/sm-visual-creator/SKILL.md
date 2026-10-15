---
name: sm-visual-creator
description: >-
  Creates platform-optimized graphics for social media posts using the Canva MCP — reads visual briefs from content drafts and generates correctly sized designs. Use when: 'create graphics', 'make the visuals', 'design my posts', 'create images for social media', 'Canva graphics', 'make the images for this week'.
---

# SM Visual Creator — Canva Graphics for Social Media

## Overview

This skill reads visual briefs from the week's content drafts (saved by sm-content-creator) and creates platform-optimized graphics using the Canva MCP. If Canva is not connected, it provides detailed manual briefs. Graphics are linked back to the content drafts in Cloud Brain so sm-schedule-queue can attach them during scheduling.

---

## Pre-Flight — Load Content Drafts & Brand

1. `mcp__cloud-brain__search_notes` with query `"drafts"` (current week's post drafts from sm-content-creator)
2. `mcp__cloud-brain__search_notes` with query `"brand snapshot"` (visual style, colors, logo status)

**If no drafts found:** *"I need this week's post drafts before creating graphics. Run sm-content-creator first."*

---

## Job Inputs (Ask Fresh Each Run)

Ask:
- **Which week's visuals are we creating?** (e.g., "week of July 7")
- **Any posts this week that already have graphics ready?** (so we skip those)
- **Do you want to review graphics before they're linked to posts, or trust the agent to proceed automatically?**

---

## Step 1 — Check Canva Connection

Attempt to connect to Canva MCP by calling `list-brand-kits`.

**If Canva is connected:** Proceed to Step 2 (automated graphic creation).

**If Canva is not connected:** 
Inform the client: *"Canva isn't connected to your agent yet. To connect it, go to Claude Settings → Connections and add the Canva MCP. Once connected, this skill will create graphics automatically.*

*In the meantime, here are detailed briefs for each post so you can create the graphics manually in Canva or your preferred design tool."*

Deliver all visual briefs from the content drafts and stop.

---

## Step 2 — Brand Kit Check

Call `list-brand-kits` to check if the client's brand kit is set up in Canva.

**If brand kit found:** Use it for all designs (colors, fonts, logo automatically applied).

**If no brand kit:** Note the brand colors and visual style from the Cloud Brain brand profile. Apply them manually to each design. Recommend: *"Setting up a Canva brand kit will make every graphic automatically match your brand. Go to Canva → Brand → Brand Kit to upload your logo and set your colors and fonts."*

---

## Step 3 — Create Graphics

For each post in the week's drafts that requires a graphic:

**Platform dimensions:**
| Platform | Format | Dimensions |
|---|---|---|
| Instagram feed (square) | Post | 1080 × 1080 |
| Instagram feed (portrait) | Post | 1080 × 1350 |
| Instagram / Facebook Story | Story | 1080 × 1920 |
| TikTok / YouTube Short | Vertical video cover | 1080 × 1920 |
| LinkedIn | Post image | 1200 × 627 |
| Facebook | Post image | 1200 × 630 |
| YouTube thumbnail | Thumbnail | 1280 × 720 |

**For each graphic, call `generate-design` or `generate-design-structured` with:**
- The correct dimensions for the platform
- The visual brief from the content draft (background, main element, text overlay, style)
- Brand colors from the brand profile
- Brand kit ID if available

**Design principles to apply:**
- Maximum 2 fonts per graphic
- Text must be legible at mobile size (minimum 24pt for body text)
- High contrast between text and background
- Brand colors as primary palette
- Leave breathing room (don't crowd the frame)
- Text overlay should be 5 words or fewer on Instagram/TikTok graphics

After creating each design, call `get-design-thumbnail` to verify it looks correct before proceeding.

---

## Step 4 — Export Designs

For each completed design, call `export-design` to get the download URL or file.

Record the asset link for each post.

---

## Step 5 — Update Drafts in Cloud Brain

Update the content drafts note with the visual asset for each post:

```
mcp__cloud-brain__write_note
path: brain/social-media/[business-name-slug]-drafts-[YYYY-MM-DD].md
```

Add to each post entry:
- `Visual: [Canva link or asset URL]`
- `Visual status: READY`

---

## Output Summary

Present a visual status report:

---

**Visuals Created — [Business Name] — Week of [date]**

| Post | Platform | Status | Link |
|---|---|---|---|
| [topic] | Instagram | ✅ Ready | [link] |
| [topic] | LinkedIn | ✅ Ready | [link] |
| [topic] | TikTok | ✅ Ready | [link] |
| [topic] | Facebook | ✅ Ready | [link] |

**Posts without graphics (text-only):** [list]
**Posts needing your video:** [list — with export specs]

---

*All graphics are linked to your post drafts. Run sm-schedule-queue next to review everything and schedule.*

---

## If Canva Is Not Connected — Manual Brief Output

For each post, present:

---

**VISUAL BRIEF — [Platform] — [Topic]**
**Dimensions:** [W × H px]
**Background:** [color/photo description]
**Main element:** [what to show — product, person, icon, etc.]
**Text overlay:** [exact text, if any — keep under 5 words for graphics]
**Colors:** [primary] / [secondary]
**Style:** [clean minimal / bold / warm / etc.]
**Font weight:** [light / regular / bold]

**To create:** Open Canva → Create design → Enter custom dimensions → Apply your brand colors → [design instructions] → Export as PNG

*Once your graphic is ready, upload it to Metricool (Planner → New Post → attach) and tell your agent: "Graphics for [week] are uploaded."*
