---
name: mkt-landing-page
description: >-
  Generate complete landing page copy and structure from an offer and audience. Produces headline, subhead, hero, value props, social proof slots, FAQ, primary CTA, and risk reversal — using your brand voice from brand-toolkit. Supports lead-capture, sales-page, waitlist, webinar-registration, and pricing-page variants. Can run from scratch or from a CRO audit's fix list.
---

# Landing Page Generator

## Overview

Writes a complete landing page (copy + structure + component blocks) for a single offer. Pulls voice and visual cues from `brand-toolkit` if installed. Supports 5 page variants. Output is markdown the user can hand to a developer or paste into a no-code builder.

Pairs with `mkt-cro-audit` — if an audit identified weak sections, this skill can rewrite just those sections.

## When This Skill Applies

- User wants a landing page written ("write a landing page for {offer}", "I need a sales page")
- User has an offer and wants copy ("draft a waitlist page for Warrior Thoughts")
- User has a CRO audit and wants the weak sections rewritten
- User mentions: landing page, sales page, lead capture page, waitlist page, webinar registration page

## Pre-Flight — Preferences

1. Search Cloud Brain: `search_notes` for `"marketing engineering preferences"` in `brain/preferences`
2. Search Cloud Brain: `search_notes` for `"brand kit"` in `brain/brand` (from brand-toolkit if installed)
3. **If brand kit found:** Load voice rules, colors, fonts. Use throughout.
4. **If preferences missing:** Ask in ONE message:
   - The offer (1-2 sentences)
   - Target audience (specific — title + situation, not "everyone")
   - Primary outcome promise (what they get, by when)
   - Top 3 objections you hear
   - Existing testimonials / case studies (have them or no?)
   - Conversion goal (book call / buy / signup / waitlist / register)
   - Save to Cloud Brain.

Banner:
```
🎯 Landing Page | Offer: {offer} | Variant: {variant} | Brand voice: {loaded/default}
```

## How It Works

### Step 1: Resolve Inputs

If `--from-audit` is passed:
- Read the CRO audit note
- Pull the weak dimensions and recommended fixes
- Rewrite ONLY the corresponding sections (don't replace the whole page)

Otherwise: gather offer, audience, variant, goal from args or by asking.

### Step 2: Apply Variant Rules

Each variant has structural requirements:

**Lead-capture:**
- Single CTA, lead magnet promise, short form (≤3 fields), single-page
- No nav, no exit links
- Hero + 3 benefits + form + 3-5 social proof items

**Sales page (long-form):**
- Lead with the problem story
- Stack: problem → agitation → solution → proof → offer → bonuses → guarantee → urgency → CTA repeats
- 1500-3000 words

**Waitlist:**
- Outcome promise + scarcity + email-only form
- Mention what subscribers get (early access, founding pricing, exclusive content)
- Light social proof

**Webinar registration:**
- Date / time prominent
- What they'll learn (3-5 bullets)
- Speaker bio
- Replay policy (live only? replay sent?)
- Reminder opt-in

**Pricing page:**
- 3-tier (or 2-tier with "best value" highlight)
- Annual/monthly toggle if SaaS
- Feature comparison table
- FAQ on billing, refunds, switching
- Risk reversal prominent

### Step 3: Generate the Sections

For the chosen variant, generate each section. Each section follows these rules:
- Headline: outcome-focused, specific, ≤12 words
- Subhead: clarifies WHO it's for and WHAT they get, ≤20 words
- Value props: 3-5 bullets, each starts with a verb, names the outcome
- Social proof slots: leave `{{testimonial-1}}` placeholders if user doesn't have testimonials yet
- CTAs: action verb + outcome ("Get my brief" not "Submit"), max 4 words
- FAQ: cover top 3 objections explicitly, plus billing/timing/refund if relevant

### Step 4: Voice Application

Apply brand voice from `brand-toolkit` if installed (detected via the `brain/brand/brand-kit` Cloud Brain note from step 2):
- Pacing (short punchy sentences vs. flowing)
- Vocabulary (avoid words flagged as off-brand)
- Personality (formal / casual / authoritative / playful)
- Owner voice rule from the bot's instructions, for example: "Direct. No fluff. Lead with data."

### Step 5: Output

- **title:** `Landing Page — {offer slug} — {variant} — {YYYY-MM-DD}`
- **folder:** `brain/marketing/pages`
- **tags:** `["landing-page", "{variant}", "{offer-slug}"]`

Note includes:
- Section-by-section copy
- Component spec (Hero / Features / Testimonials / CTA / etc.)
- Color and font notes from brand kit
- Suggested hero image direction

## Data Structure

```markdown
# Landing Page — {Offer} — {Variant}

> **Generated:** {YYYY-MM-DD}
> **Variant:** {lead-capture / sales / waitlist / webinar / pricing}
> **Conversion Goal:** {goal}
> **Brand voice:** {applied / default}

## Above the Fold

### Headline
{headline copy}

### Subhead
{subhead copy}

### Primary CTA
Button text: "{CTA verb + outcome}"
Behavior: {opens modal / scrolls to form / goes to URL}

### Hero Visual
{description — what should the hero show; e.g., "Screenshot of dashboard with concrete numbers visible. Avoid stock photos. If video, ≤30s, autoplay muted."}

## Value Props (Three Bullets)

1. **{Outcome verb} {specific outcome}** — {1 supporting sentence}
2. **{Outcome verb} {specific outcome}** — {1 supporting sentence}
3. **{Outcome verb} {specific outcome}** — {1 supporting sentence}

## Social Proof Section

### Headline
"{Trust headline — e.g., 'Trusted by 1,200+ operators'}"

### Slots
- {{logo-1}} {{logo-2}} {{logo-3}} {{logo-4}} {{logo-5}} {{logo-6}}
- Testimonial slot 1: {{name}}, {{title}}, "{quote pattern: 1 specific outcome in their own words}"
- Testimonial slot 2: {same pattern}
- Testimonial slot 3: {same pattern}
- Numbers: "{N} customers • {M} revenue tracked • {K} hours saved"

## Body

### {Section 2 heading — depends on variant}
{copy}

### {Section 3 heading}
{copy}

(More sections for sales-page / pricing variants)

## FAQ

**Q: {top objection 1}**
A: {direct answer, 2-3 sentences}

**Q: {top objection 2}**
A: {direct answer}

**Q: {top objection 3}**
A: {direct answer}

**Q: What if I want a refund?**
A: {refund policy}

**Q: How quickly will I see results?**
A: {expectation-setting answer}

## Risk Reversal

{Guarantee / free trial / cancel-anytime statement — specific terms}

## Final CTA

### Headline
"{Closing headline — recap promise}"

### Button
"{CTA — same as primary}"

### Microcopy below button
"{1 line — no credit card / start in 60 seconds / etc.}"

## Component Spec (For Developer)

- Hero: full-width, brand-color-1 background, hero image right-aligned on desktop, stacked on mobile
- Value props: 3-column on desktop, 1-column stacked on mobile
- Testimonials: carousel or 3-card row, photos required
- FAQ: accordion, all-closed by default
- CTA: solid {brand-primary} background, white text, full-width on mobile
- Font sizes: H1 {brand H1}, H2 {brand H2}, body {brand body}
- Buttons use {brand button styles}

## Suggested A/B Tests

1. Headline variant — outcome-focused vs. identity-focused
2. CTA button color — brand-primary vs. complement
3. Social proof position — above or below value props

## Notes

- Hero copy assumes audience knows {assumed knowledge}. Tighter audience may not.
- Long-form variant only: add a story section above the offer. The owner's personal voice will trim it.
```

## Output Format (Chat)

```
✓ LANDING PAGE — {offer}
Variant: {variant} • Goal: {goal}

HEADLINE
"{headline}"

SUBHEAD
"{subhead}"

PRIMARY CTA
"{cta}"

VALUE PROPS
1. {value prop}
2. {value prop}
3. {value prop}

(Full copy + dev spec saved to brain/marketing/pages/...)
```

## Example Usage

**User:** "Write a waitlist landing page for Warrior Thoughts"

**AI:** Loads Genie brand kit (owner voice). Variant: waitlist. Generates outcome-focused headline ("Mental fitness for the people who can't afford a bad mind"), short form, scarcity ("Founding 1000 — closing when full"), light social proof slots. Saves.

**User:** "/mkt-landing-page --from-audit 'CRO Audit — the provider — 2026-06-20'"

**AI:** Reads audit. Identifies weak headline + weak social proof section. Rewrites just those two sections. Outputs replacement copy with markdown showing exactly what to swap.

**User:** "Sales page for my real estate masterclass — long form"

**AI:** Variant: sales. Generates 2000-word page with problem→agitation→solution→offer→bonuses→guarantee structure. The owner's personal voice + brand kit applied. Saves.

**User:** "Pricing page for Genie plugin marketplace — 3 tiers"

**AI:** Variant: pricing. Generates 3-tier card structure, feature comparison table, FAQ on billing. Pre-fills with marketplace economics if known.

## Error Handling

- **If brand kit isn't set up:** Use defaults but flag clearly: "No brand kit found. I'm using neutral voice + standard layout. Run `/brand-toolkit-brand-kit` for branded output."
- **If offer is too vague:** Refuse to draft. Ask for one concrete outcome the offer delivers. Vague offers → vague pages → bad conversion.
- **If audience is "anyone" / "everyone":** Push back: "Landing pages that target everyone convert no one. Who specifically — job title + situation?"
- **If there are no real testimonials and user is okay placeholding:** Use `{{testimonial-1}}` placeholder slots — DO NOT fabricate testimonials. Note: "Replace placeholders with real testimonials before launching."
- **If the variant is unclear (could be lead-capture OR sales):** Ask once: "Lead capture (short, free download / book call) or sales page (long-form, direct buy)?"
- **If user has both `brand-toolkit` voice rules AND CLAUDE.md style preferences:** Brand kit wins for the page (it's the brand the page represents), but flag any conflicts: "Your brand voice differs from your personal voice. Used brand voice for the page — confirm if you want it more like your personal voice."
- **If `--from-audit` references a non-existent audit:** List recent audits and ask which.
- **If user wants the page in a specific framework (Next.js, Webflow, Notion):** Suggest the spec is framework-agnostic markdown. Offer to translate into MDX or Webflow CSS variables in a follow-up.

## See Also

- `/mkt-cro-audit` — audit this page after it goes live (same plugin)
- `/mkt-paywall-popup` — add the email-capture / exit-popup layer (same plugin)
- `/mkt-ai-seo` — make the page citable by LLMs (same plugin)
- `/brandtoolkit-brand-kit` — set up the brand voice this skill reads (from `brand-toolkit`)
- `/sm-content-batch` — schedule launch posts after the page is live (from `social-media-manager`)
