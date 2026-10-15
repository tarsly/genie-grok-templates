---
name: pro-article-illustrate
description: >-
  Generate illustrations for an article, blog post, or piece of content. Reads the source (URL, Cloud Brain note, or pasted text), picks 1-N illustration moments (hero + section breaks), generates each via available image-generation MCP (DALL-E, Midjourney, Imagen, Replicate, or local SDXL), and returns image files + caption + alt text. Uses brand-toolkit colors and visual style if installed.
---

# Article Illustrate

## Overview

Turn any article into a visual asset set. Reads the source, identifies 1-N illustration "moments" (hero image + key section transitions), generates each image, returns paths and metadata. Provider-agnostic — works with whichever image-generation MCP is installed.

Uses `brand-toolkit` for color palette + visual style consistency if installed (detected via the `brain/brand/brand-kit` Cloud Brain note).

## When This Skill Applies

- User has a blog post / article / case study and needs visuals
- User mentions: illustration, hero image, blog images, article cover, visual for this post
- User is publishing content and wants images at scale
- User says: "illustrate this", "make an image for the blog", "hero image"

## Pre-Flight — Setup Check

Detect available image-generation MCPs:
- `mcp__dalle__*` (OpenAI)
- `mcp__midjourney__*`
- `mcp__imagen__*` (Google)
- `mcp__replicate__*` (any model)
- `mcp__stability__*` (Stable Diffusion)
- Generic `mcp__image_gen__*`

If none available, suggest setup:
```
Image generation needs at least one of:
- OpenAI DALL-E MCP (recommended for ease)
- Replicate MCP (most flexible, many models)
- Stability AI MCP
- Local SDXL via ComfyUI MCP

Install one, then re-run.
```

Banner:
```
🎯 Article Illustrate | Generator: {detected} | Brand style: {loaded/default}
```

## How It Works

### Step 1: Load Source

Three input modes:
- `--source <url>` → WebFetch the URL
- `--source <note title>` → `read_note` from Cloud Brain
- `--source <text>` → use the text directly (multi-line accepted)

If user passes no source, ask for one.

### Step 2: Analyze for Illustration Moments

Read the content. Identify:
- The hero / overall metaphor (one image that captures the whole)
- Section breaks (each major H2 or theme transition is a candidate)
- Concrete imagery the article mentions (people, objects, scenes — easiest to illustrate)
- Abstract concepts (harder; need metaphor)

Cap at `--count` (default: hero only = 1; or 3-5 for full illustration set).

### Step 3: Generate Image Prompts

For each illustration moment, generate a prompt that includes:
- Subject / scene description
- Composition (e.g., "isometric", "flat illustration", "photorealistic")
- Color palette (from `brand-toolkit` if installed, else editorial neutrals)
- Style (from brand-toolkit visual style, or `--style` override, or default "clean editorial illustration")
- Avoid list (text in image, faces if avoiding likeness, etc.)
- Aspect ratio per `--aspect`

Example prompt:
```
Editorial illustration of a small business owner reviewing a tax return at a wooden desk,
late-afternoon warm light, isometric composition, brand palette: navy + warm gold,
flat shapes with soft gradients, no text, 16:9 aspect
```

### Step 4: Call Image Generator

For each prompt:
- Invoke detected MCP
- Save returned image(s) to `~/Pictures/Genie-Illustrations/{slug}-{N}.png`
- Capture metadata (model, seed if returned, prompt used)

### Step 5: Generate Captions + Alt Text

For each image:
- 1-line caption (what the image shows + relevance to article)
- Alt text (descriptive, accessibility-first, ≤125 chars)

### Step 6: Save

- **title:** `Illustrations — {article slug} — {YYYY-MM-DD}`
- **folder:** `knowledge-base/illustrations`
- **tags:** `["illustration", "{source-type}"]`

Include in the note:
- Source link / title
- Each image with path + prompt + caption + alt text
- Variations if multiple generated per moment

## Data Structure

```markdown
# Illustrations — {Article} — {YYYY-MM-DD}

> **Source:** {URL or note title}
> **Generator:** {DALL-E / Midjourney / etc.}
> **Style:** {style description}
> **Brand palette:** {applied / default}

## Hero Image

![Hero](path/to/hero.png)

- **Path:** `~/Pictures/Genie-Illustrations/{slug}-hero.png`
- **Prompt:** {full prompt used}
- **Caption:** "{1-line caption}"
- **Alt text:** "{descriptive alt}"

## Section Illustrations

### Section: {section title}

![{section}](path/to/{slug}-1.png)

- **Path:** {path}
- **Prompt:** {prompt}
- **Caption:** "{caption}"
- **Alt text:** "{alt}"

### Section: {section title}

(more...)

## Variations Generated

For each illustration moment, {N} variations were created. Picked variants above are first-choice. Other variants saved with `-v2`, `-v3` suffixes.

## Usage

- Hero image: `<img src="..." alt="..." />`
- Section images: place at start of each H2 section
- Social: use hero cropped to 1:1 for IG, 16:9 for LinkedIn, 9:16 for Reels
```

## Output Format (Chat)

```
🎨 ILLUSTRATIONS — {article title}
Generator: {tool} • Style: {style} • Aspect: {ratio}

GENERATED {N} IMAGES
1. Hero: ~/Pictures/Genie-Illustrations/{slug}-hero.png
   Caption: "{caption}"
2. {section}: {path}
3. {section}: {path}

PROMPTS USED
(saved in note for reproducibility)

Saved: knowledge-base/illustrations/Illustrations — {slug} — {date}
```

## Example Usage

**User:** "Illustrate my latest blog post" [pastes URL]

**AI:** Fetches URL. Identifies hero moment + 3 section breaks. Generates 4 images. Uses Genie brand palette (navy + gold) from brand-toolkit. Saves all 4 + metadata.

**User:** "/pro-article-illustrate --source 'Knowledge Base note title' --count 1 --aspect 1:1 --style 'minimalist line art'"

**AI:** Pulls Cloud Brain note. Generates 1 hero image in line-art style, square aspect. Saves.

**User:** "Make me 3 Instagram images for this article about Genie plugins" [pastes text]

**AI:** Analyzes text. Generates 3 images at 4:5 (Instagram-optimized). Different angles on the same article. Saves.

**User:** "Article illustration — but make it look like a New Yorker cover"

**AI:** Honors style preference. Uses style description in the prompt. Generates with appropriate model (DALL-E does this well).

## Error Handling

- **If no image-gen MCP is installed:** Cannot proceed. Suggest install options. Offer to generate the prompts only (user runs through DALL-E web).
- **If image-gen MCP returns an error (content policy, rate limit, failed generation):** Try a softer prompt rephrasing if content policy; retry once if rate-limit; report otherwise.
- **If user asks for likenesses of real people:** Warn — most APIs reject. Suggest stylized / abstracted representation.
- **If aspect ratio isn't supported by the model:** Pick closest supported, note the substitution.
- **If multiple variations are returned per generation:** Save all, mark the first as primary. Let user pick favorites later.
- **If article is very short / has no clear sections:** Default to hero-only. Don't pad with arbitrary illustrations.
- **If brand-toolkit isn't installed:** Use a neutral palette. Note: "No brand kit found — used editorial neutrals. Install `brand-toolkit` for branded visuals."
- **If MCP is slow (>30s per image):** Notify user, run in foreground with status updates. Don't time out silently.
- **If user wants commercial-use guarantees:** Note model-specific terms — DALL-E/OpenAI allow commercial use of user-generated content; Midjourney requires Pro plan; Replicate/Stability vary by model. Recommend checking before publishing.

## See Also

- `/pro-daily-gift` — uses this skill for illustration-type gifts (same plugin)
- `/brandtoolkit-brand-kit` — provides the palette and visual style (from `brand-toolkit`)
- `/mkt-landing-page` — illustrations belong on landing pages this generates (from `marketing-engineering`)
