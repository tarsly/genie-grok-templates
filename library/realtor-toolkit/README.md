# Real Estate Agent Pro — Plugin README

**Version:** 1.2.0  
**Publisher:** the provider  
**Compatible with:** Claude Cowork

---

## What This Plugin Does

Real Estate Agent Pro is a complete AI toolkit for real estate agents — from listing launch through closing, plus business development, lead nurturing, and production planning. Every skill saves your name, brokerage, and market details to memory on first use, so every subsequent output is pre-branded with zero setup.

Install once. Run it on your first real deal. Get a result that makes you look like a top producer.

---

## What's New in v1.2.0

- **11 new skills added** — listing presentations, expired/FSBO outreach, seller net sheets, offer writing and analysis, buyer lead nurture sequences, geographic farming, market reports, past client nurture, closing gifts and review requests, GCI production planning, and objection roleplay coaching
- **Preferences layer added to all 11 original skills** — every skill now searches memory for your saved details, confirms them, and runs a one-time setup interview if needed
- **Critical memory path fixes** — three original skills had hardcoded session paths that would fail on any client install; all replaced with proper Basic Memory MCP calls using standard `brain/` paths
- **Regulated disclaimers added** where required (CMA, creative finance, 1031 exchange, transaction coordination)
- **AGENTS.md** — a capabilities manifest for use with the agent designer (system-03) and agent activator (system-04)

---

## Skills — Complete List (23 total)

### Listing & Seller Side

| Skill | What It Does | Trigger Phrases |
|---|---|---|
| **listing-presentation-builder** | Full pre-listing packet, 9-step presentation system, marketing plan, pricing bridge, commission defense, 6 objection handlers, 4 close scripts | listing presentation, pre-listing, win the listing, listing appointment |
| **listing-launch-checklist** | Complete launch workflow from signed agreement to MLS live — photography brief, staging checklist, MLS input sheet, pre-launch timeline | new listing, listing checklist, listing launch, photography brief |
| **cma-builder** | Full CMA from subject property intake through comp analysis, adjustment calculation, value conclusion, pricing strategy, and seller presentation package | CMA, comparative market analysis, what should I list for, home valuation, comps |
| **price-reduction-advisor** | Data-backed price reduction analysis, seller conversation script, objection handlers, visual one-pager | price reduction, stale listing, no offers, DOM too high, cut the price |
| **seller-net-sheet-calculator** | Full net sheet with all cost categories, 3-scenario side-by-side table, carrying cost analysis | net sheet, seller proceeds, how much will I net, closing costs seller |
| **expired-fsbo-outreach** | 5-touch expired sequence + 4-touch FSBO sequence, psychology-first framing, full call and voicemail scripts | expired listing, FSBO, for sale by owner, expired outreach, door knock script |

### Buyer Side

| Skill | What It Does | Trigger Phrases |
|---|---|---|
| **buyer-consultation-builder** | Complete buyer consult package — NAR settlement context, agency explainer, buying process timeline, pre-approval checklist, discovery questions | buyer consult, new buyer, buyer presentation, buyer rep agreement |
| **showing-scheduler** | Showing itineraries, feedback requests, seller debrief reports | schedule showings, showing feedback, buyer tour, showing itinerary |
| **offer-writer-analyzer** | Buyer mode: escalation, cover letter, contingency strategy. Listing agent mode: multi-offer comparison table, risk scoring, counter-offer strategy | write an offer, analyze offers, multiple offers, offer comparison, counter offer |
| **buyer-lead-nurture** | 4-segment nurture (Hot/Warm/Cool/Long-Term), day-by-day sequences, re-engagement scripts, pipeline report | nurture my buyers, buyer follow-up sequence, buyer pipeline, long-term buyer |

### Transaction Management

| Skill | What It Does | Trigger Phrases |
|---|---|---|
| **transaction-coordinator** | Deadline tracking with RED/YELLOW/GREEN urgency, document checklist, communication drafts to lender/title/agents, pre-closing checklist | under contract, transaction, contingency deadline, TC, deal status, closing checklist |
| **disclosure-compliance-tracker** | Jurisdiction-aware disclosure checklists, document status tracking, HOA document requests, compliance audits | disclosure, SPDS, compliance, HOA documents, lead paint, disclosure deadline |

### Open House & Showings

| Skill | What It Does | Trigger Phrases |
|---|---|---|
| **open-house-optimizer** | Full open house lifecycle — prep, sign-in sheet, talking points, lead segmentation, follow-up sequences, seller debrief | open house, open house prep, open house debrief, visitor feedback |

### Business Development

| Skill | What It Does | Trigger Phrases |
|---|---|---|
| **geographic-farming-planner** | Farm economics, 12-month direct mail calendar, door-knock scripts, community presence strategy, ROI tracking | farm, geographic farm, neighborhood farm, direct mail, door knocking |
| **market-report-generator** | 15-stat data collection, trend analysis, email and print formats, A/B subject lines | market report, market update, market stats, months of supply |
| **referral-partner-manager** | Log referrals, calculate fees, relationship warmth tracking, thank-you messages, quarterly touch-base, performance report | referral, referral fee, referral partner, who sent me leads, touch base |
| **past-client-nurture** | Client capture at close, touchpoint sequence (1-week through annual), referral ask weaving, anniversary alerts | past client, client nurture, stay in touch, client anniversary |
| **closing-gift-review-requester** | Gift profiles by client type, personalized note templates, 3-window review request timing | closing gift, review request, Google review, Zillow review, gift ideas |

### Creative Finance

| Skill | What It Does | Trigger Phrases |
|---|---|---|
| **creative-vs-traditional-analyzer** | Seller candidate scoring, side-by-side comparison, installment sale tax analysis, agent compensation explainer | creative vs traditional, seller finance candidate, subject-to candidate, installment sale |
| **creative-deal-marketer** | Buyer identification and marketing channels for subject-to, seller finance, lease-option deals | market a creative deal, find buyers for seller finance, sub2 marketing |

### Specialized

| Skill | What It Does | Trigger Phrases |
|---|---|---|
| **1031-exchange-tracker** | Day 45/Day 180 deadline tracking, replacement property log, compliance status, QI communications | 1031, tax deferred exchange, like-kind exchange, replacement property, QI |
| **gci-tracker-production-planner** | Reverse pipeline math from GCI goal, production dashboard, deal logging, monthly progress, recalibration | GCI goal, production planner, how many deals do I need, income goal |
| **objection-roleplay-coach** | Practice mode, scripts library, custom scenario mode — 10 fully scripted objections with psychology, 3-part framework, coaching rubric | objection roleplay, practice objections, objection scripts, commission objection |

---

## How Preferences Work

Every skill runs a one-time setup on first use:

1. Searches memory for `realtor-toolkit preferences`
2. If found → displays your saved details and confirms
3. If not found → runs a quick setup interview (name, brokerage, market, and any skill-specific defaults)
4. Saves to `brain/preferences/realtor-toolkit-preferences.md`
5. Shows a preferences banner on every output:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Agent: [Your Name] | [Brokerage] | Market: [Area]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

To update your preferences at any time, tell any skill: *"Update my preferences"* and it will re-run the setup interview.

---

## Memory Paths (Standard)

All data is saved to Basic Memory MCP under these paths:

| Data Type | Path |
|---|---|
| Agent preferences | `brain/preferences/realtor-toolkit-preferences.md` |
| Transactions | `brain/transactions/[address-slug].md` |
| 1031 exchanges | `brain/transactions/1031-[client-slug]-[YYYY-MM-DD].md` |
| CMA outputs | `brain/deal-analyses/[address-slug]-cma-[YYYY-MM-DD].md` |
| Disclosures | `brain/disclosures/[address-slug].md` |
| Referral partners | `brain/referrals/referral-partners.md` |
| Buyer leads | `brain/leads/buyer-[name-slug].md` |
| Past clients | `brain/clients/[name-slug].md` |
| Geographic farms | `brain/projects/farm-[neighborhood-slug].md` |
| GCI tracker | `brain/projects/gci-tracker-[year].md` |
| Market reports | `brain/projects/market-report-[area-slug]-[YYYY-MM].md` |

---

## Two-Phase Design

Skills that involve high-stakes analysis follow a two-phase pattern:

- **Phase 1 (Screener/Builder)** — collects inputs, runs analysis, saves output to memory
- **Phase 2 (Advanced analysis)** — reads Phase 1 output from memory automatically, no re-entry required

Example: `cma-builder` saves the comp grid and value conclusion to `brain/deal-analyses/` → the listing presentation can reference it during the seller meeting.

---

## Using with the Agent System (system-03 / system-04)

This plugin includes `AGENTS.md` — a capabilities manifest that tells the agent designer (system-03) what this plugin can do, what preferences are needed, and how to suggest agent configurations for real estate clients.

**At system-03 (agent design):** If system-03 detects a real estate agent client, it can suggest org chart configurations — from a single all-in-one assistant to a full team of specialized agents.

**At system-04 (agent activation):** When activating a real estate agent, system-04 will run the preferences setup for the skills assigned to that agent. Preferences do not need to be set up separately — they are part of the activation flow.

See `AGENTS.md` in this plugin folder for the full capabilities reference, recommended schedules, and suggested agent configurations.

---

## Disclaimer

Skills in this plugin provide professional workflow tools for licensed real estate agents. They do not constitute legal advice, financial advice, or licensed appraisals. CMA outputs are pricing estimates, not USPAP-compliant valuations. Creative finance analysis is educational — always verify tax implications with a CPA and legal structure with a real estate attorney. 1031 exchange tracking is a deadline management tool — consult a Qualified Intermediary and tax professional for compliance decisions. Disclosure requirements vary by state — always verify with your broker and a licensed real estate attorney.

---

*the provider — Real Estate Agent Pro v1.2.0*


## Grok Bot import
See the root README. Replace `{{BOT_NAME}}` in `system-prompt.md` and `workflows/getting-started/SKILL.md`.
