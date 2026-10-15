# Brand Intelligence — Agent Capabilities Manifest

*For use by system-03 (Agent Designer) and system-04 (Agent Activator)*
*Last updated: 2026-06-12*

---

## About This Plugin

`brand-toolkit` gives small business owners and entrepreneurs a complete brand setup and measurement system. The Brand Kit skill captures identity data once and saves it to Cloud Brain — from that point forward, every document and content skill is automatically branded. The Brand Audit skill measures how the brand actually lands online, finds gaps, benchmarks against competitors, and builds a 30-day improvement plan.

**Target users:** Solopreneurs, small business owners, real estate agents, coaches, consultants — anyone who markets themselves or their business online and wants to show up professionally and consistently.

**Plugin version:** 1.0.0

---

## Available Skills Catalog

---

### 1. MKT-brand-kit

**What it does:** Guides the client through a brand setup interview (identity, contact info, social handles, colors, fonts, logo, and brand voice), saves everything to Cloud Brain as the MKT preferences record, and generates a visual designer-style brand board PDF with Polaroid color swatches, font specimens, and a full-width color bar.

**Preferences collected at activation:**
- Full name and professional title
- Company or brand name
- Tagline and elevator pitch
- Industry and niche
- Contact info (email, phone, website)
- Social media handles (Instagram, TikTok, YouTube, LinkedIn, Facebook, Threads, others)
- Brand colors (primary, secondary, accent, text, background)
- Typography (heading font, body font, script/signature font — file upload supported)
- Logo files (primary, white/reversed, icon — file upload supported)
- Brand voice (tone, personality adjectives, target audience, what to avoid)
- Document defaults (logo in header, footer text, legal disclaimer)

**Suggested schedule:** On-demand — run once at setup, then again when the brand evolves.

**Natural pairings:** MKT-brand-audit (audit reads brand preferences saved here), social-media-pro skills (all read MKT preferences for context), docx/pptx/pdf document skills (apply brand colors and logo automatically).

---

### 2. MKT-brand-audit

**What it does:** Researches the client's digital presence across social media, website, and Google search. Produces a scored brand snapshot, gap analysis (where the brand is vs. where it needs to be), competitive positioning comparison, 30-day improvement plan, content pillar recommendations, and platform-specific bio and tagline suggestions. Compares against prior audits when available to show progress or regression.

**Preferences collected at activation:**
- Loads from brand kit preferences (no re-entry required if brand kit is set up)
- Job inputs asked fresh each run: social handles/URLs being audited, specific goals for this audit period

**Suggested schedule:** Quarterly — brand health check every 90 days.

**Natural pairings:** MKT-brand-kit (audit reads the brand identity set up here), MKT-social-calendar (audit content pillars feed directly into the social calendar).

---

## Preferences Registry

All identity preferences for this plugin are stored at a single Cloud Brain path:

```
brain/preferences/mkt-preferences.md
```

All skills in this plugin read from and write to this path. Setting up the brand kit once makes both skills immediately context-aware.

**Fields stored:**
- Full name, title, company name, tagline, elevator pitch, industry/niche
- Contact info (email, phone, website, city/state)
- Social handles (Instagram, TikTok, YouTube, LinkedIn, Facebook, Threads, other)
- Brand colors (primary, secondary, accent, text, background)
- Typography (font names and file paths)
- Brand voice (tone, personality adjectives, target audience, avoid)
- Document defaults
- Asset paths (brand JSON location, logo locations)

---

## Suggested Agent Configuration

**Brand & Marketing Agent (single agent — recommended)**

Assign both skills to one "Brand & Marketing" agent. This is the natural setup for this plugin — both skills share preferences and are used by the same person for the same purpose.

**Activation fields:** All fields in the Preferences Registry above. Takes approximately 10 minutes once. After setup, both skills are pre-loaded and the client never re-enters their business identity.

**Activation tip:** system-04 should run the Brand Kit skill as part of activation — this both collects preferences AND produces the brand board as a deliverable, so the client immediately sees value from the setup session.

---

## Recommended Schedules

| Skill | Recommended Schedule | Notes |
|---|---|---|
| MKT-brand-kit | On-demand | Set up once; re-run when brand evolves or client wants to update colors/fonts/logo |
| MKT-brand-audit | Quarterly | Brand health check every 90 days; compares to prior audit automatically |

---

## How system-03 Should Use This Document

When a client indicates they run a business with a public-facing brand, market themselves online, or need help with marketing and content, system-03 should:

1. Surface `brand-toolkit` as a foundational setup plugin — it creates the identity layer all content and document skills draw from
2. Recommend running MKT-brand-kit before any content or document skill is used
3. Suggest a quarterly cadence for MKT-brand-audit as a recurring accountability check

---

## How system-04 Should Use This Document

When activating a Brand & Marketing agent with this plugin:

1. Check whether MKT preferences already exist: `mcp__cloud-brain__search_notes: "MKT preferences"`
2. If found: confirm with the client and skip the interview — preferences carry forward
3. If not found: run MKT-brand-kit as the activation interview — it collects all preferences AND delivers the brand board PDF as a first deliverable
4. Write all preferences to `brain/preferences/mkt-preferences.md`
5. After activation, give the client the trigger phrases for each skill so they know how to use them
