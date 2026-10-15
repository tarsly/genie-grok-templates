---
name: mkt-brand-kit
description: >-
  Brand kit setup — create, edit, or view a named brand profile with guided interview covering identity, contact info, social handles, colors, fonts, logo, and brand voice. Generates a visual designer-style brand board PDF with Polaroid color swatches, font specimens, social icons, and color bar. Brand data auto-applies to all document skills (docx, pptx, xlsx, pdf, email-drafter, etc.). Use when user says 'set up my brand', 'brand kit', 'brand setup', 'add a new brand', 'edit my brand', 'update my colors/logo/fonts', 'show my brand', 'brand guide', 'brand board', 'brand profile', 'brand style guide', or any request to define or update brand identity. Invoke immediately when a document skill needs brand data and none is loaded.
---

# MKT-brand-kit — Brand Kit Setup & Management

## Overview

Creates and manages named brand profiles. This skill is the **identity source of truth** for the MKT domain — every other skill in the `brand-toolkit` and `social-media-pro` plugins reads from what is set up here.

Brand data is saved in two places:
1. **Cloud Brain** (`brain/preferences/mkt-preferences.md`) — the preferences record all skills read at startup
2. **Local assets folder** — logo files, font files, and the full JSON for document skill consumption

The brand board output is a **visual designer board** — Polaroid-style color swatches, large signature/script name treatment, font specimens, social icons, and a full-width color bar. One page. Not a brochure.

---

## Step 1: Check for Existing Brand

Search Cloud Brain before starting:

```
mcp__cloud-brain__search_notes: query = "MKT preferences"
```

- **Brand found** → load it, confirm in one line: *"Working as [Name] at [Business] — edit, view, or regenerate brand board?"*
  - **Edit** → go to Step 2B
  - **View** → display brand summary and offer to regenerate board
  - **Regenerate** → skip to Step 4 with existing data
- **No brand found** → go to Step 2A (New Brand Interview)

---

## Step 2A: New Brand Interview

Tell the user: *"I'll walk you through building your brand kit a few questions at a time — everything can be updated later."*

Ask in blocks. Wait for answers before moving on.

### Block 1 — Brand Name & Identity
1. What would you like to name this brand? (e.g., "My Agency", "Personal", "Main Business")
2. Your full name as it appears professionally
3. Your title (e.g., CEO, REALTOR®, Real Estate Investor, Founder)
4. Company or brand name
5. Tagline or slogan (optional — skip if none yet)
6. One-sentence elevator pitch: who do you help and how?
7. Industry or niche (e.g., real estate, coaching, e-commerce, consulting)

### Block 2 — Contact Info
8. Email address (for documents)
9. Phone number
10. Website URL (optional)
11. City and state
12. License number and state (optional — real estate, finance, etc.)

### Block 3 — Social Media
13. Instagram handle
14. Facebook page
15. LinkedIn profile URL
16. YouTube channel
17. TikTok handle
18. Threads handle (optional)
19. Any others? (Twitter/X, Pinterest, etc.)

### Block 4 — Brand Colors

Say: *"Colors work best as hex codes like #335568. If you have brand colors but not the hex codes, share them however you know them and I'll convert."*

20. Primary color
21. Secondary color
22. Accent color
23. Text color (default: #1A1A1A if not specified)
24. Background color (default: #FFFFFF if not specified)

If more than 2–3 colors are provided, save all in an `extended_palette` array.

### Block 5 — Typography & Fonts

Say exactly: *"Now for fonts. I support custom font files — if you have your brand fonts as .ttf or .otf files, please upload them now and I'll save them permanently so they render correctly in all your documents and your brand board. If you don't have the files handy, just tell me the font names and I'll note them — you can upload the files anytime later by saying 'update my brand fonts'."*

25. Heading/display font name (e.g., Kollektif, Playfair Display, Montserrat)
26. Body text font name (e.g., Roboto Slab Thin, Open Sans, Lato)
27. Signature/script font for your name or brand mark? (e.g., Dancing Script, Great Vibes — optional)

**If font files are uploaded:** Save them to the assets folder with clear names like `kollektif.ttf`, `roboto-slab-thin.ttf`. Record the paths in the brand JSON.

**Font conversion note:** If uploaded fonts are OTF with PostScript outlines, convert using fonttools:
```bash
pip install fonttools --break-system-packages -q
python3 -m fonttools otf2ttf input.otf -o output.ttf
```
If that fails, use PIL/ImageFont to pre-render large display text as PNG and embed in the PDF.

### Block 6 — Logo Files

Say: *"Do you have a logo file to upload? PNG with a transparent background works best, but SVG, AI, EPS, and JPG are all supported."*

28. Primary logo (color version)
29. White/reversed logo for dark backgrounds (optional)
30. Icon or mark only — symbol without wordmark (optional)

When logo files are uploaded (they arrive in the uploads folder):
- Save to brand assets folder with standardized names: `logo-primary.png`, `logo-white.png`, `logo-icon.png`
- For EPS/AI files, convert to PNG using Ghostscript:
  ```bash
  gs -dNOPAUSE -dBATCH -dSAFER -sDEVICE=pngalpha -r300 -dEPSCrop \
     -sOutputFile="logo-primary.png" "input.eps"
  ```

### Block 7 — Brand Voice
31. Tone description (e.g., warm and approachable, bold and energetic, professional and elevated)
32. 3–5 personality adjectives
33. Target audience — be specific (e.g., "first-time homebuyers in Phoenix", "female entrepreneurs building product businesses")
34. What to avoid — 1–2 things your brand never sounds or looks like

### Block 8 — Document Defaults
35. Include logo in document headers? (yes/no)
36. Standard footer text for documents
37. Legal disclaimer? (optional)

---

## Step 2B: Edit Mode

1. Load existing brand data from Cloud Brain
2. Show a clean summary grouped by section
3. Ask: *"What would you like to change? Tell me naturally."*
4. For font updates: *"You can upload new font files now if you have them — just drag them in."*
5. Apply changes, save (both Cloud Brain and local JSON), confirm what changed
6. Offer to regenerate the brand board

---

## Step 3: Save the Brand Data

**Slug:** lowercase, spaces → hyphens. "My Agency" → `my-agency`

### 3A — Save to Cloud Brain (primary — all skills read from here)

Save a clean preferences record at `brain/preferences/mkt-preferences.md`:

```markdown
# MKT Preferences — [Brand Name]

**Brand:** [brand_name]
**Slug:** [slug]
**Full Name:** [full_name]
**Title:** [title]
**Company:** [company_name]
**Tagline:** [tagline]
**Elevator Pitch:** [elevator_pitch]
**Industry / Niche:** [industry]
**Target Audience:** [target_audience]
**City / State:** [city_state]

## Contact
- Email: [email]
- Phone: [phone]
- Website: [website]

## Social Platforms
- Instagram: [handle]
- Facebook: [page]
- LinkedIn: [url]
- YouTube: [channel]
- TikTok: [handle]
- Threads: [handle]
- Other: [other]

## Brand Colors
- Primary: [hex]
- Secondary: [hex]
- Accent: [hex]
- Text: [hex]
- Background: [hex]

## Typography
- Heading Font: [name]
- Body Font: [name]
- Script Font: [name]

## Brand Voice
- Tone: [tone]
- Personality: [adjectives]
- Avoid: [avoid]

## Document Defaults
- Include Logo in Header: [yes/no]
- Footer Text: [footer]
- Legal Disclaimer: [disclaimer]

## Asset Paths (local)
- Brand JSON: [path]
- Logo Primary: [path]
- Last Updated: [YYYY-MM-DD]
```

Use `mcp__cloud-brain__write_note` to save.

### 3B — Save full JSON locally (for document skills and brand board generation)

```bash
mkdir -p /mnt/.claude/brain/brands
mkdir -p /mnt/.claude/assets/brands/{slug}
mkdir -p /mnt/.claude/assets/fonts
```

Save the full brand JSON at `/mnt/.claude/brain/brands/brand-{slug}.json`:

```json
{
  "brand_name": "",
  "slug": "",
  "created_at": "YYYY-MM-DD",
  "updated_at": "YYYY-MM-DD",
  "identity": {
    "full_name": "",
    "title": "",
    "company_name": "",
    "tagline": "",
    "elevator_pitch": "",
    "industry": "",
    "license_number": "",
    "license_state": "",
    "city_state": ""
  },
  "contact": {
    "email": "",
    "phone": "",
    "website": ""
  },
  "social": {
    "instagram": "",
    "facebook": "",
    "linkedin": "",
    "youtube": "",
    "tiktok": "",
    "threads": "",
    "other": []
  },
  "colors": {
    "primary": "",
    "secondary": "",
    "accent": "",
    "text": "#1A1A1A",
    "background": "#FFFFFF",
    "extended_palette": []
  },
  "typography": {
    "heading_font": "",
    "body_font": "",
    "script_font": "",
    "font_files": {
      "heading": "",
      "body": "",
      "script": ""
    }
  },
  "assets": {
    "logo_primary": "",
    "logo_white": "",
    "logo_icon": ""
  },
  "voice": {
    "tone": "",
    "personality_adjectives": [],
    "target_audience": "",
    "avoid": []
  },
  "document_defaults": {
    "include_logo_in_header": true,
    "footer_text": "",
    "legal_disclaimer": ""
  }
}
```

---

## Step 4: Generate the Brand Board PDF

After saving, generate the brand board using the bundled script included in this plugin at `scripts/generate_brand_board.py`.

```bash
pip install reportlab Pillow fonttools --break-system-packages -q
python3 scripts/generate_brand_board.py /mnt/.claude/brain/brands/brand-{slug}.json
```

The script handles:
- Font loading (TTF/OTF) and fallbacks to system fonts
- Logo embedding (PNG/JPG; EPS converted to PNG before passing)
- Script font rendering via PIL for large display text
- Polaroid swatch layout with drop shadows
- All brand colors, typography, voice, and contact sections
- Output saved to the outputs folder as `brand-{slug}-brand-board.pdf`

### Brand Board Design (the script follows this spec)

The board feels like a designer brand board, not a document:
- **Background:** warm off-white or light tone pulled from the brand palette
- **Left column:** 5 Polaroid-style color swatches — white frame, thick white footer strip with color name + hex, drop shadow, staggered 3-2 grid
- **Right column top:** Brand name in script/signature font (large), then name + brand in heading font, then tagline in italic
- **Right column middle:** Horizontal color bar strip, then typography panels (heading + body font specimens in light gray panels)
- **Right column bottom:** Brand voice adjective pills (using brand colors), contact info, social platform icons
- **Bottom:** Full-width color bar with color names, footer label in small caps

---

## Step 5: Confirm and Present

```
✅ Brand "[Name]" saved to Cloud Brain — all MKT skills are now pre-loaded
📄 Brand board saved to your outputs folder
```

*"This brand is now available to all your document and content skills. I'll automatically apply your colors, fonts, logo, and contact info to any document I create — unless you tell me to use a different brand."*

Present the brand board PDF using `present_files`.

---

## How Other Skills Load Brand Preferences

At startup, any MKT skill that needs brand context should:

```
1. mcp__cloud-brain__search_notes: query = "MKT preferences"
2. If found: load the note, extract needed fields
3. Confirm in one line: "Working as [Name] at [Business], correct?"
4. If not found: prompt user to run MKT-brand-kit first
```

For skills that need the full JSON (e.g., document generation with logo embedding), load locally:

```python
import json, os, glob

def load_brand_json(brand_slug=None):
    brand_dir = "/mnt/.claude/brain/brands/"
    if not os.path.exists(brand_dir):
        return None
    if brand_slug:
        path = f"{brand_dir}brand-{brand_slug}.json"
    else:
        files = [f for f in glob.glob(f"{brand_dir}brand-*.json")
                 if 'style-guide' not in f]
        if not files:
            return None
        path = sorted(files)[0]
    return json.load(open(path)) if os.path.exists(path) else None
```

---

## Edge Cases

- **Color name without hex** — suggest a hex, confirm before saving
- **No logo** — use brand name in large heading font as placeholder in the board
- **Font file is OTF with CFF outlines** — try `fonttools otf2ttf`; if that fails, pre-render display text as PNG via PIL
- **Brand name collision** — ask: overwrite or new name?
- **Partial interview** — save with `"status": "draft"` in JSON, note in Cloud Brain record as incomplete. Resume next time: "Your brand kit is incomplete — want to finish setting it up?"
- **Multiple brands** — Cloud Brain record stores the active brand slug. User can say "use my [name] brand" to switch. List all brands with: `search_notes: "MKT preferences"`
- **User only has brand name, no details** — save what they have, mark draft, ask for remaining fields in the next conversation

## Update Preferences

If the user says "update my brand" or "change my [brand element]":
1. Load from Cloud Brain
2. Ask what changed
3. Apply changes to both Cloud Brain record and local JSON
4. Offer to regenerate brand board
5. Confirm: "Updated. Your [element] is now [new value] — applied everywhere."

## See Also

- `/MKT-brand-audit` — quarterly brand audit reads this brand kit (same plugin)
- `/mkt-landing-page` — generates landing pages using this brand voice and palette (from `marketing-engineering`)
- `/mkt-paywall-popup` — popups in your brand colors (from `marketing-engineering`)
- `/pro-screen-polish` — intro/outro cards use these colors and logo (from `prosumer-ops`)
- `/pro-article-illustrate` — AI illustrations in your palette (from `prosumer-ops`)
