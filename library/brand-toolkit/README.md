# Brand Intelligence Plugin

**Version:** 1.0.0 | **Author:** the provider | **Tier:** Standard

Set up your brand identity once. Measure it quarterly. Show up consistently everywhere.

---

## What This Plugin Does

Brand Intelligence gives you two tools that work together as a system:

**Brand Kit** captures who you are, what you look like, and how you sound — then saves it to your AI brain so every document, presentation, and piece of content you create is automatically on-brand. Run it once and your name, logo, colors, and voice are pre-loaded forever.

**Brand Audit** measures how your brand is actually landing online. It searches your social channels, website, and Google presence, scores what it finds, compares you to competitors, and builds you a 30-day plan to close the gaps. Run it every quarter and it shows you whether you're improving.

---

## Skills in This Plugin

### MKT-brand-kit — Brand Setup & Brand Board
*Trigger phrases: "set up my brand", "brand kit", "brand setup", "brand board", "update my colors", "update my logo", "show my brand", "brand style guide"*

A guided interview that captures your brand identity in 8 blocks:
- Brand name, title, tagline, and elevator pitch
- Contact info and website
- Social media handles (Instagram, TikTok, YouTube, LinkedIn, Facebook, Threads)
- Brand colors — accepts hex codes or color names (you'll be prompted to confirm hex before saving)
- Typography — font names, with optional font file upload for pixel-perfect rendering
- Logo files — upload PNG, SVG, EPS, or AI; the skill handles conversion
- Brand voice — tone, personality adjectives, target audience, what to avoid
- Document defaults — logo in headers, footer text, legal disclaimers

After setup, you get a **visual brand board PDF** — Polaroid-style color swatches, font specimens, your logo, social icons, and a full-width color bar. Everything a designer would put on a brand guide, on one page.

**Run this first.** Every other skill in this plugin (and your document skills) reads from what's set up here.

### MKT-brand-audit — Full Brand Analysis
*Trigger phrases: "audit my brand", "brand audit", "review my online presence", "brand health check", "how does my brand look?", "brand gap analysis", "30-day brand plan", "content pillars"*

A research-backed audit of your brand across every channel:

- **Brand Snapshot** — an honest assessment of how your brand reads to a stranger, with platform-by-platform scores
- **Gap Analysis** — where you are vs. where you need to be across authority, visibility, trust, and clarity
- **Competitive Positioning** — how you compare to 3–5 competitors in your space, and where the open positioning is
- **30-Day Improvement Plan** — a day-by-day action plan, ~45 minutes/day, for fixing the biggest gaps
- **Content Pillar Recommendations** — the 3–5 core topics your brand should consistently post about
- **Bio & Tagline Suggestions** — platform-specific bios and 5 tagline options

**Run this quarterly.** Prior audits are saved, so each new audit shows you the score trend — improving or regressing — since your last check.

---

## How They Work Together

```
Run Brand Kit (once)
    ↓
Brand identity saved to your Cloud Brain
    ↓
Brand Audit reads your saved identity automatically — no re-entry
    ↓
Audit finds gaps, recommends content pillars
    ↓
Content pillars feed into your Social Media Calendar (social-media-pro plugin)
    ↓
Run Brand Audit again in 90 days — see if the score improved
```

---

## Two-Phase System

| Phase | Skill | When to Run | What You Get |
|---|---|---|---|
| Setup | MKT-brand-kit | Once, then when brand evolves | Brand board PDF, preferences saved |
| Quarterly check | MKT-brand-audit | Every 90 days | Scored audit, improvement plan, progress vs. last audit |

---

## Updating Your Preferences

Your brand details are saved in your Cloud Brain. To update:
- "Update my brand" — edit any field
- "Update my colors" — change primary, secondary, or accent hex codes
- "Update my logo" — upload a new logo file
- "Update my fonts" — upload new font files or change font names
- "Regenerate my brand board" — produce a fresh PDF with current data

---

## What's New in v1.0.0

- Initial release
- Full brand kit setup with Cloud Brain persistence
- Brand board PDF generation (Polaroid swatches, font specimens, social icons)
- Brand audit with 8-section output and 30-day improvement plan
- Quarterly re-audit with progress comparison
- Threads platform support
- Content pillar recommendations integrated with social-media-pro plugin

---

*Brand Intelligence is part of the the provider plugin marketplace. Install via the provider.*


## Grok Bot import
See the root README. Replace `{{BOT_NAME}}` in `system-prompt.md` and `workflows/getting-started/SKILL.md`.
