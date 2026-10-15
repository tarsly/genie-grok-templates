---
name: mkt-paywall-popup
description: >-
  Generate copy + behavior spec for paywalls, popups, exit intent modals, signup gates, banner bars, slide-ins, and toast notifications. Each component includes trigger logic, frequency cap, A/B variants, fallback text for users who dismiss, and accessibility considerations. Output is hand-off-ready for any popup tool (ConvertBox, OptinMonster, Wisepops, custom code).
---

# Paywall & Popup Generator

## Overview

Writes copy AND specifies behavior for the popup/paywall/modal layer of a site. Generates 6 component types, each with trigger logic, frequency cap, A/B variants, and dismiss-fallback copy. Output is tool-agnostic — paste into ConvertBox, OptinMonster, Wisepops, or hand to a developer.

Pairs with `mkt-cro-audit` — if an audit identifies need for a popup, this skill executes it.

## When This Skill Applies

- User wants a paywall or signup gate ("write a paywall", "set up an exit popup", "draft a signup modal")
- User has a lead magnet and wants the modal copy
- User is launching a content site and needs the email-capture popup
- User mentions: paywall, popup, modal, exit intent, signup gate, banner bar, slide-in, lead magnet, email capture

## Pre-Flight — Preferences

Loads `marketing engineering preferences`. Asks one additional question on first run:
- Popup frequency policy — how often will the same visitor see popups? (once per session / once per week / once per visit / aggressive)
- Brand-tone for interruptions — apologetic / direct / playful / urgent

Banner:
```
🎯 Popup | Type: {type} | Goal: {goal} | Frequency cap: {policy}
```

## How It Works

### Step 1: Pick the Type

| Type | Best for | Trigger | Friction |
|------|----------|---------|----------|
| **Paywall** | Premium content unlock | After N free articles or scroll % | Highest — blocks content |
| **Exit popup** | Recover abandoning visitors | Cursor leaves viewport (desktop) / scroll back (mobile) | Medium |
| **Signup gate** | Email-required content | Before content access | High |
| **Banner bar** | Site-wide promo / announcement | Always visible until dismissed | Low |
| **Slide-in** | Soft offer, low interruption | Scroll % or time on page | Low |
| **Toast** | Notifications, confirmations | Event-based | Very low |

### Step 2: Define the Offer

For each type, what's the value being exchanged?
- Paywall: subscription / one-time unlock
- Exit popup: discount, lead magnet, last-chance reminder
- Signup gate: lead magnet, gated content, free trial
- Banner: announcement, urgency, navigation cue
- Slide-in: lead magnet, related content, soft pitch
- Toast: social proof ("X just signed up"), system status

### Step 3: Write Copy

For each component, write:

- **Headline** (5-10 words) — outcome-focused
- **Body** (1-3 sentences) — the value
- **CTA button** (2-4 words) — action verb
- **Dismiss text** (3-7 words) — soft, not guilt-trippy
- **Fine print** (optional) — privacy / cancel / unsubscribe note
- **Fallback** — what shows after dismiss for users who said no

### Step 4: Specify Behavior

For each component, define:
- Trigger (time / scroll / exit intent / event)
- Frequency cap (per session / per day / per week / per visitor lifetime)
- Conditions (page URL contains X, source = Y, has cookie Z, etc.)
- Animation (fade / slide / instant)
- Z-index and stacking rules
- Dismiss behavior (close + remember / close + cookie / close + show fallback)
- Mobile vs. desktop variants

### Step 5: Generate A/B Variants

For the primary copy, generate 2-3 variants for A/B testing:
- Variant A: outcome-focused
- Variant B: identity-focused ("for {audience}")
- Variant C: urgency-focused (if honest urgency exists)

### Step 6: Accessibility

For each component, specify:
- Keyboard escape (Esc closes the modal)
- Focus trap (Tab cycles within modal)
- ARIA labels (dialog role, labelledby)
- Color contrast (≥4.5:1 for text)
- Animation respects `prefers-reduced-motion`
- Dismiss button is reachable for screen readers

### Step 7: Save

- **title:** `Popup — {type} — {offer slug} — {YYYY-MM-DD}`
- **folder:** `brain/marketing/pages`
- **tags:** `["popup", "{type}", "{offer-slug}"]`

## Data Structure

```markdown
# Popup — {Type} — {Offer}

> **Generated:** {YYYY-MM-DD}
> **Type:** {type}
> **Goal:** {goal — email capture / unlock / subscribe / dismiss}
> **Audience:** {audience}

## Copy

### Variant A (Outcome-focused)

**Headline:** "{headline}"
**Body:** {body}
**CTA:** "{cta button}"
**Dismiss:** "{dismiss}"
**Fine print:** "{fine print}"

### Variant B (Identity-focused)

**Headline:** "{headline}"
**Body:** {body}
**CTA:** "{cta button}"
**Dismiss:** "{dismiss}"
**Fine print:** "{fine print}"

### Variant C (Urgency, if honest urgency exists)

...

## Behavior Spec

| Setting | Value |
|---------|-------|
| Trigger | {time / scroll / exit / event} |
| Trigger value | {30s / 50% / etc.} |
| Frequency cap | {once per session / per 7 days / per visitor} |
| Conditions | {URL contains / source / cookie / etc.} |
| Animation | {fade / slide / instant} |
| Dismiss behavior | {close + cookie / close + show fallback / close + show again on next page} |
| Mobile variant | {same / smaller / banner instead / disabled} |

## Form (if applicable)

- Fields required: {list — minimize to email-only when possible}
- Validation: {what's checked client-side}
- Submit behavior: {redirect / inline success / show next step}
- Email destination: {ESP — Mailchimp / Beehiiv / ConvertKit / Substack}
- Success message: "{1 sentence — what they get next}"

## Fallback (Post-Dismiss)

What shows for users who dismissed:
- Type: {smaller banner / nothing / different offer}
- Copy: "{1 sentence — soft}"
- Reappearance: {next session / next week / never}

## Accessibility

- ARIA: `role="dialog"`, `aria-labelledby="{headline-id}"`, `aria-describedby="{body-id}"`
- Focus trap: yes — Tab cycles within modal
- Keyboard escape: Esc closes modal
- Dismiss button: large enough for touch (≥44x44px), labeled "Close" for screen readers
- Color contrast: ≥4.5:1 text-to-background
- Reduced motion: animations disabled if `prefers-reduced-motion: reduce`

## A/B Test Plan

- **Primary metric:** {conversion rate / email captures / signups}
- **Guardrail metric:** {bounce rate / time on site / subsequent conversions}
- **Traffic target:** {N per variant}
- **Runtime estimate:** {days}
- **Stop conditions:** {confidence reached / time elapsed / spike in guardrail}

## Implementation Notes

Tool-specific snippets (provide on request):
- ConvertBox: {config}
- OptinMonster: {config}
- Custom code: {React/JS snippet}
```

## Output Format (Chat)

```
✓ POPUP — {type}
Goal: {goal}

VARIANT A (RECOMMENDED)
H: "{headline}"
B: {body}
CTA: "{cta}"

VARIANT B
H: "{headline}"
CTA: "{cta}"

TRIGGER
{trigger}, cap: {frequency cap}

A/B TEST
Primary: {metric} • Runtime: {days}

Full spec: brain/marketing/pages/Popup — {type} — {offer} — {date}
```

## Example Usage

**User:** "Write an exit popup for Warrior Thoughts waitlist"

**AI:** Type: exit-popup. Goal: email capture for waitlist. Generates 3 variants (outcome / identity / scarcity since waitlist is naturally scarce). Specifies trigger (exit intent desktop, scroll-back mobile), frequency (once per 7 days). Saves.

**User:** "/mkt-paywall-popup --type paywall --goal 'subscription' --offer 'Pro features' --audience 'small business owners'"

**AI:** Generates paywall copy. Includes "5 free articles per month" pattern, soft fallback ("sign up to read more"), and hard fallback ("subscribe to continue"). Specifies frequency: once per article view.

**User:** "Banner bar for Genie announcing the EOS plugin launch"

**AI:** Type: banner. Goal: traffic to plugin page. Generates copy with urgency (launch week). Specifies dismiss + 30-day cookie. Mobile: full-width sticky bottom bar.

**User:** "I have a free PDF lead magnet — set up a slide-in popup"

**AI:** Type: slide-in. Trigger: 50% scroll. Copy: focuses on the PDF outcome ("Get the 5-step framework"). Form: email-only. Frequency: once per session.

## Error Handling

- **If user wants an aggressive popup pattern (paywall after 1 article, exit popup every visit):** Push back gently — patterns this aggressive backfire long-term. Suggest moderation. If user insists, do it but note the risk in the spec.
- **If brand voice is heavy and the popup pattern is light (banner / toast):** Trim voice to fit the constraint. Banners need to be scannable in <2s.
- **If the offer is weak ("subscribe to our newsletter"):** Push back — popups exchange interruption for value. If value isn't there, conversion rates will be low. Suggest improving the offer first.
- **If user has no ESP set up:** Note: "You need an email service provider (Beehiiv / ConvertKit / Mailchimp) wired up before this works. Recommend Beehiiv for newsletters, ConvertKit for solopreneurs."
- **If user wants to skip dismissal (no close button):** Refuse. Dark patterns get penalized by browsers and lose long-term trust.
- **If popup conflicts with cookie consent (EU):** Sequence them — consent first, then popup. Note in spec.
- **If user is targeting mobile-heavy audience:** Default to slide-in or banner over full modal — mobile modals are friction-heavy.
- **If `mkt-cro-audit` flagged need for the popup:** Read the audit note from `brain/marketing/audits/` and tailor the copy to address the specific weak dimensions the audit identified.

## See Also

- `/mkt-cro-audit` — identify which popups your page actually needs (same plugin)
- `/mkt-landing-page` — landing page that this popup overlays (same plugin)
- `/brandtoolkit-brand-kit` — colors and voice the popup uses (from `brand-toolkit`)
