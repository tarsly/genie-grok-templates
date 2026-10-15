# Real Estate Agent Pro — Agent Capabilities Manifest

*For use by system-03 (Agent Designer) and system-04 (Agent Activator)*
*Last updated: 2026-05-22*

---

## About This Plugin

`realtor-toolkit` is a complete AI toolkit for licensed real estate agents. It covers every major workflow in a productive agent's business: listing and transaction management, buyer representation, lead generation and nurture, market analysis, business development, compliance, and coaching. Skills save preferences to shared memory so the agent's name, brokerage, market, and business details are pre-loaded across every interaction.

**Target users:** Solo agents, small teams, and buyer/listing agents at any production level. Especially valuable for agents looking to systematize their business, improve their listing-to-close conversion, and grow their referral pipeline.

**Plugin version:** 1.1.0 (18 skills at release; 5 added in v1.2.0)

---

## Available Skills Catalog

Each entry lists: what the skill does, what preferences it collects at activation, its recommended schedule, and which other skills it pairs with naturally.

---

### 1. listing-launch-checklist

**What it does:** Generates a complete pre-listing and go-live checklist for every new listing — photography, signage, MLS input, marketing activation, disclosure preparation, lockbox, and showing setup. Nothing falls through the cracks at launch.

**Preferences collected at activation:**
- Agent name, brokerage, market
- MLS platform used
- Standard listing go-live lead time (days before live date)
- Preferred photographer contact
- Preferred sign company / sign inventory status
- Preferred lockbox type (combo / electronic / Supra)

**Suggested schedule:** On-demand — triggered when a listing is signed.

**Natural pairings:** cma-builder (pricing is done before launch), disclosure-compliance-tracker (disclosures must be ready at launch), listing-presentation-builder (won the listing first).

---

### 2. cma-builder

**What it does:** Builds a complete Comparative Market Analysis from subject property intake through comp selection, adjustment grid, value conclusion, and a seller presentation package with pricing strategy and objection handlers for the Zillow conversation.

**Preferences collected at activation:**
- Agent name, brokerage, market
- Typical comp window in the market (days — e.g., 90)
- Default condition adjustment ranges (or use skill defaults)
- Local market factors (hot/slow, appreciation trend)

**Suggested schedule:** On-demand — triggered before any listing appointment or when a seller asks "what is my home worth?"

**Natural pairings:** listing-presentation-builder (CMA feeds directly into the presentation), seller-net-sheet-calculator (net sheet uses the CMA price), price-reduction-advisor (references original CMA when recommending a reduction).

---

### 3. transaction-coordinator

**What it does:** Tracks every open transaction from accepted offer to close. Monitors contingency deadlines with traffic-light urgency (red if due within 3 days), tracks required documents, drafts nudge communications to lender, title, and cooperating agents, and generates pre-closing checklists.

**Preferences collected at activation:**
- Agent name, brokerage, market
- Default inspection deadline (days from acceptance)
- Default appraisal deadline (days from acceptance)
- Default loan/financing deadline (days from acceptance)
- Preferred lender partner (name, phone, email)
- Preferred title/escrow officer (name, phone, email)

**Suggested schedule:** Daily (morning) — deadline check every morning on all open transactions.

**Natural pairings:** disclosure-compliance-tracker (run together as a morning compliance check), showing-scheduler (coordinates with TC timeline for active listings).

---

### 4. price-reduction-advisor

**What it does:** Analyzes showing activity, days on market, and market conditions to recommend whether and when to reduce price. Prepares a seller communication package with the data-driven case for the reduction and scripts for the conversation.

**Preferences collected at activation:**
- Agent name, brokerage, market
- Average DOM in market before a price review is warranted
- Typical showing-to-offer ratio in market

**Suggested schedule:** On-demand — triggered when a listing goes stale or agent wants to check if a reduction is warranted.

**Natural pairings:** cma-builder (original CMA provides the pricing baseline), showing-scheduler (showing feedback informs the reduction recommendation), listing-presentation-builder (if repositioning is significant enough to warrant a new seller meeting).

---

### 5. open-house-optimizer

**What it does:** Builds a complete open house plan — marketing timeline, sign placement, registration system, conversation scripts for visitors, follow-up sequences for attendees, and a post-event debrief report for the seller.

**Preferences collected at activation:**
- Agent name, brokerage, market
- Preferred open house registration method (paper, QR code, app)
- Default follow-up cadence after open house (same day / next day / 3 days)

**Suggested schedule:** On-demand — triggered when scheduling or preparing for an open house.

**Natural pairings:** showing-scheduler (manage the showing calendar around open houses), buyer-lead-nurture (open house visitors feed the buyer lead pipeline), market-report-generator (open house is a distribution channel for market reports).

---

### 6. buyer-consultation-builder

**What it does:** Generates a complete buyer consultation package for new buyer clients — welcome agenda, local market snapshot, buying process timeline, agency agreement explainer, how-we-work-together overview, mortgage pre-approval checklist, and discovery questions. Post-NAR settlement context makes the consultation more important than ever for explaining buyer representation.

**Preferences collected at activation:**
- Agent name, brokerage, market
- Buyer representation fee structure and how agent explains the agreement
- Preferred lender partner (to include in pre-approval checklist)
- Average buyer timeline in market (months from first showing to close)

**Suggested schedule:** On-demand — triggered before any new buyer meeting.

**Natural pairings:** showing-scheduler (buyer consult leads directly to scheduling showings), offer-writer-analyzer (when the buyer is ready to write), buyer-lead-nurture (for buyers who aren't ready yet after the consult).

---

### 7. showing-scheduler

**What it does:** Two-mode showing tool. Buyer agent mode: builds optimized showing itineraries, drafts MLS showing requests, sends buyer confirmation messages, creates property comparison worksheets. Listing agent mode: sends professional feedback requests after each showing, compiles buyer sentiment reports, generates seller debrief presentations.

**Preferences collected at activation:**
- Agent name, brokerage, market
- Showing service used (ShowingTime, CSS, other, or direct)
- Default feedback request timing (hours after showing)
- Preferred property comparison format

**Suggested schedule:** On-demand — triggered when scheduling buyer tours or following up on listing showings.

**Natural pairings:** buyer-consultation-builder (tours follow the buyer consult), offer-writer-analyzer (showing feedback informs offer strategy), price-reduction-advisor (compiled showing feedback supports reduction recommendations).

---

### 8. disclosure-compliance-tracker

**What it does:** Generates jurisdiction-aware disclosure checklists for every listing, tracks document status, flags overdue items, drafts request messages to sellers/HOAs/buyers, and runs on-demand compliance audits. Missing a disclosure deadline creates legal liability and can kill a deal.

**Preferences collected at activation:**
- Agent name, brokerage, market / state
- State-specific required disclosures (agent provides or skill uses standard checklist)
- HOA document deadline requirements
- Brokerage disclosure review requirement (does broker review before delivery?)

**Suggested schedule:** Daily (morning) — run alongside transaction-coordinator as a combined compliance check on all active transactions.

**Natural pairings:** transaction-coordinator (shares the same transaction timeline), listing-launch-checklist (disclosures must be ready at launch day).

---

### 9. creative-deal-marketer

**What it does:** Generates marketing materials, buyer outreach scripts, and social media content for creative finance deals (subject-to, seller financing, wrap mortgages, lease options). Creative deals require a different marketing approach — this skill targets the right buyer audience with the right language.

**Preferences collected at activation:**
- Agent name, brokerage, market
- Brokerage approval status for creative finance transactions
- Active creative finance community or investor network memberships
- Social media platforms used for marketing

**Suggested schedule:** On-demand — triggered when marketing a creative finance deal.

**Natural pairings:** creative-vs-traditional-analyzer (analysis precedes marketing), expired-fsbo-outreach (expired and FSBO sellers are prime creative finance candidates), offer-writer-analyzer (creative offers require non-standard terms).

---

### 10. creative-vs-traditional-analyzer

**What it does:** Side-by-side analysis of a creative finance structure (subject-to, seller carry, lease option) versus a traditional sale for a specific property and seller situation. Presents numbers, pros/cons, and risk factors so the seller can make an informed decision.

**Preferences collected at activation:**
- Agent name, brokerage, market
- Brokerage approval for creative finance discussions
- Agent's familiarity level with creative finance (affects depth of explanations generated)

**Suggested schedule:** On-demand — triggered when a seller situation suggests creative finance may be worth analyzing.

**Natural pairings:** creative-deal-marketer (analysis leads to marketing if seller chooses creative route), seller-net-sheet-calculator (traditional net sheet is one side of the comparison), cma-builder (market value underlies the analysis).

---

### 11. referral-partner-manager

**What it does:** Tracks the referral network — who sent what leads, conversion rates, fees owed at close, relationship warmth, and partner contact cadence. Generates weekly relationship maintenance lists, calculates referral fees, and drafts personalized thank-you messages. 30–50% of top agent business comes from referrals.

**Preferences collected at activation:**
- Agent name, brokerage, market
- Default referral fee percentage (for outbound referrals)
- Preferred referral fee payment method
- Preferred CRM platform (for cross-reference)
- Weekly relationship maintenance day preference

**Suggested schedule:** Weekly — Monday morning maintenance list of partners to touch.

**Natural pairings:** past-client-nurture (past clients are the most valuable source of referrals), gci-tracker-production-planner (referral volume is a key lead source metric), closing-gift-review-requester (referral partners deserve recognition when they send business).

---

### 12. 1031-exchange-tracker

**What it does:** Mission-critical deadline tracking for 1031 tax-deferred exchanges. Captures exchange setup, calculates Day 45 (identification) and Day 180 (close) deadlines, tracks identified replacement properties, generates status dashboards, creates alert messages, and drafts QI communications. Missing a deadline costs clients massive capital gains taxes.

**Preferences collected at activation:**
- Agent name, brokerage, market
- Preferred QI (Qualified Intermediary) contact(s) — name, phone, email
- Default identification rule recommendation (3-property / 200% / 95%)
- Whether agent works with an exchange-specialist attorney or CPA to refer

**Suggested schedule:** Daily when active exchanges exist — deadline monitoring is time-critical.

**Natural pairings:** transaction-coordinator (the replacement property close is a transaction that needs TC tracking), seller-net-sheet-calculator (relinquished property net proceeds feed into exchange math), offer-writer-analyzer (replacement property offer timing is critical).

---

### 13. listing-presentation-builder

**What it does:** Builds a complete listing appointment presentation package — pre-listing research, marketing plan, agent value proposition, track record framing, commission justification, and objection handlers for the toughest seller questions. Use before any listing appointment.

**Preferences collected at activation:**
- Agent name, brokerage, market
- Agent's track record stats (list-to-sale ratio, avg DOM, units closed)
- Marketing plan components available (professional photography, 3D tour, targeted ads, etc.)
- Commission rate and value proposition summary

**Suggested schedule:** On-demand — triggered before any listing appointment.

**Natural pairings:** cma-builder (CMA is presented inside the listing presentation), seller-net-sheet-calculator (net sheet is a core part of the listing package), objection-roleplay-coach (prep for the listing appointment with roleplay).

---

### 14. expired-fsbo-outreach

**What it does:** Multi-touch outreach sequences for expired listings and FSBOs — letters, text scripts, call scripts, email sequences, and door-knock approaches tailored to each prospect's specific situation. Covers both frustrated expired sellers and self-represented FSBO sellers.

**Preferences collected at activation:**
- Agent name, brokerage, market
- Preferred outreach channels (direct mail / phone / door knock / all)
- Touch cadence preference (aggressive / moderate / light)
- Value proposition differentiator for expired vs. FSBO conversations

**Suggested schedule:** On-demand — triggered when prospecting expired listings or FSBOs.

**Natural pairings:** listing-presentation-builder (expired/FSBO outreach leads to listing appointments), cma-builder (provide a CMA as a prospecting hook), objection-roleplay-coach (practice FSBO objection #4 before calling).

---

### 15. seller-net-sheet-calculator

**What it does:** Fast seller net proceeds calculator — what does the seller actually walk away with after commissions, closing costs, liens, repairs, and prorated expenses? Runs multiple scenarios (list high / at market / to sell fast) side by side so sellers make informed pricing decisions.

**Preferences collected at activation:**
- Agent name, brokerage, market
- Typical closing cost percentages in the market (transfer taxes, escrow fees, title insurance)
- Commission rate
- Common seller concession amounts in current market

**Suggested schedule:** On-demand — triggered at listing appointments, price reduction conversations, and any time a seller asks "how much will I net?"

**Natural pairings:** cma-builder (CMA price feeds the net sheet), listing-presentation-builder (net sheet is a core presentation component), creative-vs-traditional-analyzer (traditional net is one side of the creative vs. traditional comparison).

---

### 16. offer-writer-analyzer

**What it does:** Two-mode offer tool. Buyer agent mode: writes a competitive offer strategy with escalation clauses, cover letter, contingency recommendations, and seller-friendly terms. Listing agent mode: analyzes multiple offers side by side and generates a seller presentation for an informed decision.

**Preferences collected at activation:**
- Agent name, brokerage, market
- Typical contingency periods in market (inspection, appraisal, financing)
- Current market competitiveness (multiple offers common / not common)
- Preferred cover letter tone (warm and personal / professional and clean)

**Suggested schedule:** On-demand — triggered when writing or reviewing a purchase offer.

**Natural pairings:** buyer-consultation-builder (offer strategy is explained during buyer consult), showing-scheduler (showing feedback informs offer strategy), transaction-coordinator (accepted offer kicks off TC tracking).

---

### 17. buyer-lead-nurture

**What it does:** Systematic nurture sequences for buyer leads at every stage — ready now, 3–6 months out, 6–12 months out, and long-term. Generates personalized email, text, and call templates for each segment with the right cadence and content. Saves leads to memory for ongoing tracking.

**Preferences collected at activation:**
- Agent name, brokerage, market
- CRM platform used (for cross-reference)
- Default nurture cadence per segment (touch frequency)
- Preferred contact channels (text / email / phone / mix)

**Suggested schedule:** Weekly — pipeline review and outreach generation for leads due for a touch.

**Natural pairings:** buyer-consultation-builder (nurtured leads eventually convert to consultation appointments), open-house-optimizer (open house attendees enter the nurture pipeline), market-report-generator (market reports are high-value nurture content for buyer leads).

---

### 18. geographic-farming-planner

**What it does:** Builds a complete 12-month geographic farming plan for a target neighborhood — content calendar, direct mail sequences, door-knock strategy, community engagement plan, and market stat inserts. Includes postcard copy, door-knock scripts, and event ideas.

**Preferences collected at activation:**
- Agent name, brokerage, market
- Target farm neighborhood(s)
- Approximate home count in farm area
- Monthly farming budget
- Preferred channels (direct mail / door knock / digital / community events)
- Current market share in farm area (estimated)

**Suggested schedule:** Quarterly — campaign planning session for the upcoming quarter.

**Natural pairings:** market-report-generator (market reports are the primary farming content), listing-launch-checklist (just listed / just sold postcards are a core farming touchpoint), expired-fsbo-outreach (farm area expired listings are high-priority prospects).

---

### 19. market-report-generator

**What it does:** Generates a professional monthly or quarterly neighborhood market report for sphere nurture, farming campaigns, and past client touchpoints. Takes raw MLS stats the agent provides and produces a polished report with trend analysis, insights, email/print formats, and social media captions.

**Preferences collected at activation:**
- Agent name, brokerage, market
- Agent phone and email (for report footer)
- Primary farm neighborhood or report area
- Distribution channels (email / print / both)
- Publish frequency (monthly / quarterly)

**Suggested schedule:** Monthly — first week of each month for monthly reports; first week of each quarter for quarterly.

**Natural pairings:** geographic-farming-planner (market report is the farming content anchor), buyer-lead-nurture (market reports are high-value content for buyer leads), past-client-nurture (reports are a recurring touchpoint for past clients).

---

### 20. past-client-nurture

**What it does:** Systematic touchpoint management for past clients — anniversary messages, home value check-ins, referral asks, and milestone follow-ups. Captures clients at close, saves them to memory, calculates anniversary dates, generates personalized messages for every touchpoint, and produces a client status report with alerts for upcoming anniversaries and overdue contacts.

**Preferences collected at activation:**
- Agent name, brokerage, market
- Agent phone and email (for message templates)
- Whether agent uses Homebot or similar automated home value service
- Non-anniversary touch frequency preference (quarterly / bi-annually / annually only)

**Suggested schedule:** Weekly check — flag upcoming anniversaries and clients overdue for contact.

**Natural pairings:** closing-gift-review-requester (immediately post-close), referral-partner-manager (past clients who refer become referral partners), market-report-generator (reports are distributed to past client list), gci-tracker-production-planner (past client referrals are a tracked lead source).

---

### 21. closing-gift-review-requester

**What it does:** Two-part post-close skill. Part 1: generates personalized closing gift recommendations (3 options at agent's budget) tailored to the client's profile, plus a handwritten note template with specific transaction references. Part 2: drafts warm, effective review request messages with timing strategy, platform-specific templates, coaching language for the client, and public review response templates.

**Preferences collected at activation:**
- Agent name, brokerage, market
- Closing gift budget per transaction
- Gift style preference (local/personalized / practical / experiences)
- Review platforms used (Google, Zillow, Realtor.com, other)
- Google review link (for auto-population in templates)
- Zillow review link (optional)

**Suggested schedule:** On-demand — triggered at close, 1 month post-close (if no review yet), and 6 months post-close (final ask).

**Natural pairings:** past-client-nurture (post-close gift and review are the first touches in the nurture sequence), referral-partner-manager (clients who leave reviews are strong referral candidates).

---

### 22. gci-tracker-production-planner

**What it does:** Annual GCI goal-setting and production math tool. Reverse-engineers an income goal into the exact number of leads, appointments, signed clients, and closes needed each month. Logs closed transactions, tracks running YTD GCI against goal, produces monthly progress reports, and recalibrates monthly targets when the agent is behind pace.

**Preferences collected at activation:**
- Agent name, brokerage, market
- Typical commission rate
- Brokerage split (percentage agent keeps)
- Average sale price in market
- Primary business focus (buyer-side / seller-side / both)

**Suggested schedule:** Weekly — Friday production check-in; also triggered when logging a new closed deal.

**Natural pairings:** referral-partner-manager (referral volume is a tracked lead source), buyer-lead-nurture (lead pipeline health is reflected in monthly targets), geographic-farming-planner (farming budget should align with production plan).

---

### 23. objection-roleplay-coach

**What it does:** Interactive objection handling practice with three modes — Practice (live roleplay with coaching feedback), Scripts Library (word-for-word scripts for all 10 core objections), and Custom Scenario (tailored prep for a specific upcoming conversation). Covers commission objections, Zillow pricing, FSBO, discount agents, buyer rep agreement refusal, appraisal gaps, and more.

**Preferences collected at activation:**
- Agent name, brokerage, market
- Commission rate (so roleplay scripts use the agent's real number)
- Primary business focus (buyer / seller / both)
- Primary coaching challenge (winning listings / price objections / buyer rep agreements / other)

**Suggested schedule:** On-demand — triggered before listing appointments, buyer consultations, or any high-stakes client conversation.

**Natural pairings:** listing-presentation-builder (practice the listing presentation objections before the appointment), buyer-consultation-builder (practice buyer rep agreement objections), cma-builder (practice Zillow objection with real CMA data in hand).

---

## Preferences Registry

All preferences that can be collected at agent activation, organized by category. system-04 should collect the union of all fields needed by the assigned skills and run one consolidated interview.

**Core Identity — needed by most skills:**
- Agent full name
- Brokerage name
- Primary market / city
- Agent phone number
- Agent email
- License number (optional — for report footers and marketing compliance)

**Listing and Sales:**
- Typical listing commission rate (%)
- Buyer agent compensation / buyer fee structure (post-NAR settlement format)
- MLS platform used
- Average DOM in market before a price review is warranted
- Typical showing-to-offer ratio in market
- Agent track record stats (list-to-sale ratio, avg DOM, units closed — for listing presentation)
- Marketing plan components available (professional photos, 3D tour, targeted digital ads, etc.)

**Transactions:**
- Default inspection deadline (days from acceptance)
- Default appraisal deadline (days from acceptance)
- Default loan / financing deadline (days from acceptance)
- Preferred lender partner name, phone, and email
- Preferred title / escrow officer name, phone, and email

**Business Operations:**
- Brokerage split (% the agent keeps)
- Average sale price in market
- Annual GCI goal (or take-home goal — skill calculates GCI)
- Primary business focus (buyer-side / seller-side / both)
- Preferred CRM platform

**Farming and Marketing:**
- Target farm neighborhood(s)
- Approximate farm area home count
- Monthly farming budget
- Preferred farming channels (direct mail / door knock / digital / community events)
- Do they use Homebot or a similar home value service? (yes/no)
- Report distribution method (email / print / both)
- Report publish frequency (monthly / quarterly)
- Social media platforms used

**Post-Close:**
- Closing gift budget per transaction
- Gift style preference (local/personalized / practical / experiences)
- Review platforms used
- Google review link
- Zillow review link

**Client Nurture:**
- Non-anniversary touch frequency for past clients (quarterly / bi-annually / annually only)

**Creative Finance:**
- Brokerage approval status for creative finance transactions
- Active creative finance community or investor network memberships

**Referrals:**
- Default outbound referral fee (%)
- Preferred referral fee payment method
- Weekly relationship maintenance day preference

**1031 Exchange:**
- Preferred QI contact(s) — name, phone, email
- Default identification rule recommendation (3-property / 200% / 95%)
- Referral attorney or CPA for exchange clients

**Coaching:**
- Primary coaching challenge (winning listings / price objections / buyer rep agreements / other)

---

## Suggested Agent Configurations

**These are starting-point suggestions only.** Clients are free to design any org structure — one agent, five agents, or anything in between. Skills can be assigned to any agent in any combination. The goal is to match the configuration to how the agent actually thinks about their work.

---

### Option A: Solo Agent (One AI Assistant)

One "Real Estate Assistant" agent handles everything. Assign all 23 skills to one agent. Activation collects all preferences in one interview.

**Best for:** Solo agents new to AI agents, agents who want simplicity over segmentation, or any client who wants to start with one agent and split later.

**Activation interview:** All fields from the Preferences Registry above. Takes approximately 10–15 minutes once. All 23 skills pre-loaded and ready.

---

### Option B: Two-Agent Setup (Most Common Starting Point)

**Listing and Transaction Agent**
Skills: cma-builder, listing-presentation-builder, listing-launch-checklist, price-reduction-advisor, seller-net-sheet-calculator, transaction-coordinator, disclosure-compliance-tracker, 1031-exchange-tracker, creative-vs-traditional-analyzer, offer-writer-analyzer (listing agent mode)

Activation fields: Core Identity, Listing/Sales, Transactions, Creative Finance

**Business Development and Nurture Agent**
Skills: buyer-consultation-builder, buyer-lead-nurture, showing-scheduler, open-house-optimizer, offer-writer-analyzer (buyer agent mode), referral-partner-manager, past-client-nurture, closing-gift-review-requester, geographic-farming-planner, market-report-generator, expired-fsbo-outreach, creative-deal-marketer, gci-tracker-production-planner, objection-roleplay-coach

Activation fields: Core Identity, Farming/Marketing, Post-Close, Client Nurture, Referrals, Business Operations, Coaching

---

### Option C: Three-Agent Team

**Listing Agent**
Skills: cma-builder, listing-presentation-builder, listing-launch-checklist, price-reduction-advisor, seller-net-sheet-calculator, disclosure-compliance-tracker, creative-vs-traditional-analyzer, objection-roleplay-coach (seller objections focus)

Activation fields: Core Identity, Listing/Sales, Coaching

**Transaction and Compliance Agent**
Skills: transaction-coordinator, disclosure-compliance-tracker, 1031-exchange-tracker, showing-scheduler, offer-writer-analyzer

Activation fields: Core Identity, Transactions, 1031 Exchange

**Business Development and Nurture Agent**
Skills: buyer-consultation-builder, buyer-lead-nurture, open-house-optimizer, referral-partner-manager, past-client-nurture, geographic-farming-planner, market-report-generator, expired-fsbo-outreach, gci-tracker-production-planner, closing-gift-review-requester, creative-deal-marketer, objection-roleplay-coach (buyer objections focus)

Activation fields: Core Identity, Farming/Marketing, Post-Close, Client Nurture, Referrals, Business Operations, Creative Finance, Coaching

---

### Option D: Full Team (4+ Agents)

For agents running larger operations or teams who want clear separation of function:

**Listing Agent** — cma-builder, listing-presentation-builder, listing-launch-checklist, price-reduction-advisor, seller-net-sheet-calculator, disclosure-compliance-tracker, creative-vs-traditional-analyzer

**Transaction and Compliance Agent** — transaction-coordinator, disclosure-compliance-tracker, 1031-exchange-tracker, showing-scheduler, offer-writer-analyzer

**Marketing Agent** — geographic-farming-planner, market-report-generator, open-house-optimizer, creative-deal-marketer, expired-fsbo-outreach

**Client Success Agent** — past-client-nurture, closing-gift-review-requester, referral-partner-manager, buyer-lead-nurture, buyer-consultation-builder, gci-tracker-production-planner, objection-roleplay-coach

---

## Recommended Schedules

For system-04 to reference when suggesting automated task cadences at activation. All schedules are suggestions — the client confirms or adjusts during activation.

| Skill | Recommended Schedule | Notes |
|---|---|---|
| transaction-coordinator | Daily, morning | Deadline check every morning on all open transactions |
| disclosure-compliance-tracker | Daily, morning | Run alongside TC — combined compliance check |
| 1031-exchange-tracker | Daily, morning (when active exchanges exist) | Time-critical — Day 45 and Day 180 deadlines are hard stops |
| referral-partner-manager | Weekly, Monday morning | Relationship maintenance list for the week |
| market-report-generator | Monthly, first week | Pull MLS stats and generate report at start of each month |
| past-client-nurture | Weekly check | Flag upcoming anniversaries and overdue contacts |
| buyer-lead-nurture | Weekly | Pipeline review and outreach generation |
| gci-tracker-production-planner | Weekly, Friday | End-of-week production check-in |
| geographic-farming-planner | Quarterly | Campaign planning for upcoming quarter |
| listing-launch-checklist | On-demand | Triggered when listing signed |
| cma-builder | On-demand | Triggered before listing appointment or valuation request |
| listing-presentation-builder | On-demand | Triggered before listing appointment |
| price-reduction-advisor | On-demand | Triggered when listing goes stale or agent requests review |
| seller-net-sheet-calculator | On-demand | Triggered at listing appointment or price conversation |
| open-house-optimizer | On-demand | Triggered when scheduling or preparing an open house |
| buyer-consultation-builder | On-demand | Triggered before new buyer meeting |
| showing-scheduler | On-demand | Triggered when scheduling tours or following up on showing feedback |
| offer-writer-analyzer | On-demand | Triggered when writing or reviewing a purchase offer |
| expired-fsbo-outreach | On-demand | Triggered when prospecting expired listings or FSBOs |
| creative-vs-traditional-analyzer | On-demand | Triggered when creative finance is worth analyzing |
| creative-deal-marketer | On-demand | Triggered when marketing a creative finance deal |
| closing-gift-review-requester | On-demand | Triggered at close, 1 month, and 6 months post-close |
| objection-roleplay-coach | On-demand | Triggered before high-stakes appointments |

---

## How system-03 Should Use This Document

When a client's brain context indicates they are a real estate agent (from system-02 business blueprint), system-03 should:

1. **Reference the Skills Catalog** to understand what capabilities are available — do not assume; check here.
2. **Surface capabilities based on what the client says they need** — not based on a fixed structure. If a client says "I need help staying in touch with past clients," point them to past-client-nurture, closing-gift-review-requester, and referral-partner-manager specifically.
3. **Present the skill catalog as a menu of capabilities** the client can assign freely to whatever agents they create — they are not locked into any configuration.
4. **Use the Suggested Configurations as conversation starters**, not defaults. Show Option A, B, C, or D and ask: "Does one of these match how you think about your business, or would you like to customize?"
5. **Check Natural Pairings** in each skill entry when designing multi-agent setups — these are the highest-value skill combinations.
6. **Note skills that share disclosure or compliance obligations** (transaction-coordinator + disclosure-compliance-tracker) — these are typically assigned together.

---

## How system-04 Should Use This Document

When activating an agent that has been assigned `realtor-toolkit` skills:

1. **Identify which skills are assigned to this agent** from the system-03 design output.
2. **Look up each assigned skill in the Skills Catalog** to get its "Preferences collected at activation" list.
3. **Take the union of all preference fields** across all assigned skills — this is the complete set to collect. Do not run a separate interview per skill.
4. **Cross-reference the Preferences Registry** to understand which fields are shared across many skills (Core Identity, Listing/Sales) versus skill-specific (1031 Exchange QI contact, closing gift budget). Collect shared fields once.
5. **Run one consolidated setup interview** — not one per skill. Group questions naturally: "Let's start with your basic info, then your business setup, then a few skill-specific questions."
6. **Write all preferences to one shared file:**
   ```
   brain/preferences/realtor-toolkit-preferences.md
   ```
   All skills in this plugin read from this single path. Writing to it once makes all skills work immediately.
7. **Reference the Recommended Schedules table** to suggest automated task cadences. Present the full schedule table to the client and ask: "Would you like any of these to run on a schedule, or would you prefer on-demand only?"
8. **Check if preferences already exist** before running the interview:
   ```
   mcp__cloud-brain__search_notes: "realtor-toolkit preferences"
   ```
   If found, confirm with the client and skip the interview — preferences carry across all agents in this plugin.
9. **After activation, confirm** which skills are live and what their schedules are. Give the client a one-line trigger phrase for each skill so they know how to use them.
