---
name: disclosure-compliance-tracker
description: >-
  Manage the disclosure and compliance process for every real estate listing — one of the most legally sensitive parts of the job. Generate jurisdiction-aware disclosure checklists, track document status for every transaction, flag overdue items, draft request messages to sellers/HOAs/buyers, and run on-demand compliance audits. Use this skill the moment a new listing is signed, whenever a disclosure deadline is approaching, or anytime you hear 'have we sent the disclosures yet?' — missing a disclosure deadline creates serious liability for the agent and can kill a deal. Triggers on: disclosure, seller disclosure, SPDS, compliance, HOA documents, lead paint, disclosure checklist, disclosure deadline, buyer disclosure packet, disclosure tracker, missing disclosures.
---

# Disclosure & Compliance Tracker

> **Disclaimer:** Disclosure requirements are legally mandated and vary significantly by state and municipality. Missing a required disclosure — or delivering one late — can expose you to license complaints, lawsuits, and deal cancellation. This skill provides a professional compliance framework and is NOT legal advice. Always verify your state's specific requirements with your broker and a licensed real estate attorney. When in doubt, disclose more, not less.

Disclosures are legally mandated. Missing one — or delivering one late — puts your license, your deal, and your professional reputation at risk. This skill is your compliance safety net, tracking every document for every listing from contract to close.

---

## STEP 0: PREFERENCES

Check memory for saved agent preferences first.

```
mcp__cloud-brain__search_notes: "realtor-toolkit preferences"
```

**If preferences found:** Confirm — "Using: [Agent Name], [Brokerage], default state: [State]. Proceed or update?"

**If not found:** Run setup interview:

> Let me save your details to make every disclosure checklist faster.
> 1. Your full name?
> 2. Brokerage name?
> 3. Your primary state (drives default disclosure requirements)?
> 4. Broker contact for compliance questions (name, phone/email)?

Save to memory:
```
mcp__cloud-brain__write_note:
  path: brain/preferences/realtor-toolkit-preferences.md
```

Show preferences banner on every output:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Agent: [Name] | [Brokerage] | State: [State]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## STEP 1: LISTING SETUP

When a new listing is signed, capture:

**Property Details:**
- Full property address
- State (verify against preferences — may differ if agent works across state lines)
- Property type: single family / condo / townhome / multi-family / land
- Year built (pre-1978 triggers federal lead-based paint disclosure)
- HOA present? (yes/no)
- Known material defects the seller has disclosed to you

Save listing to memory:
```
mcp__cloud-brain__write_note:
  path: brain/disclosures/[address-slug].md
  content: [full disclosure tracking record]
```

---

## STEP 2: GENERATE THE DISCLOSURE CHECKLIST

Based on property details, generate a complete, prioritized checklist.

### Universal (All Properties)

| Document | Required By | Status | Due | Notes |
|---|---|---|---|---|
| Seller's Property Disclosure Statement (SPDS) | State law | ⬜ Not started | Before MLS / within 3–5 days of contract | Most critical document |
| Agency Disclosure | State law | ⬜ Not started | At first contact | |
| Wire Fraud Advisory | Brokerage policy | ⬜ Not started | At listing or contract | Protect buyer/seller |
| MLS Clear Cooperation Acknowledgment | MLS rules | ⬜ Not started | Before going active | |

### Pre-1978 Homes (Lead-Based Paint)

| Document | Required By | Status | Notes |
|---|---|---|---|
| Lead-Based Paint Disclosure | Federal law (HUD/EPA) | ⬜ Not started | Required for ALL homes built before 1978 — no exceptions |
| Lead-Based Paint Pamphlet | Federal law | ⬜ Not started | Must be given to buyer with the disclosure |

### HOA Properties

| Document | Required By | Status | Notes |
|---|---|---|---|
| HOA Addendum | State law / contract | ⬜ Not started | |
| CC&Rs | State law | ⬜ Not started | Order from HOA management company |
| Current Budget & Reserve Study | State law | ⬜ Not started | Shows financial health of HOA |
| Recent Meeting Minutes (last 12 months) | State law | ⬜ Not started | |
| Pending Special Assessments | Disclosure obligation | ⬜ Not started | Must disclose if seller is aware |
| HOA Rules & Regulations | Contract | ⬜ Not started | |

### State-Specific (Verify with Broker)
- Natural Hazard Disclosure — required in CA, FL, TX, and others
- Mold Disclosure — required in several states
- Asbestos/Radon Disclosure — varies by state
- Septic/Well Disclosure — if not on public utilities
- Solar Panel Disclosure — if panels are leased vs. owned

---

## STEP 3: TRACK DOCUMENT STATUS

Five stages per document:
1. **NOT STARTED** — not yet requested
2. **IN PROGRESS** — sent to seller/HOA, awaiting return
3. **SIGNED BY SELLER** — completed and signed
4. **DELIVERED TO BUYER** — delivered to buyer's agent or buyer
5. **ACKNOWLEDGED BY BUYER** — buyer has signed receipt

Flag any document in stages 1–2 with a deadline approaching.

---

## STEP 4: DEADLINE ALERTS

| Trigger | Typical Deadline | Alert Level |
|---|---|---|
| SPDS to seller | Day 0 (at listing) | Critical |
| SPDS delivered to buyer | Within 3–5 days of contract | 🔴 RED if overdue |
| Lead paint disclosure | At time of contract | 🔴 RED if missing |
| HOA documents ordered | Day 0 (before contract) | 🟡 YELLOW approaching |
| HOA documents delivered to buyer | Within 3–5 days of contract | 🔴 RED if overdue |
| Buyer's review period | 3–5 business days after receipt | Warn as it expires |

Within 3 days of deadline: **CRITICAL** alert. Within 7 days: **WARNING**.

---

## STEP 5: DOCUMENT REQUEST DRAFTS

All drafts auto-populate agent name and brokerage from preferences.

**To Seller — Request SPDS:**
> "Hi [Seller Name], I'm attaching the Seller's Property Disclosure Statement for [address]. Please complete it as thoroughly and accurately as possible — this document protects you legally and is required by law. Focus especially on any known issues with the roof, HVAC, plumbing, electrical, or foundation, even if repaired. I need this back within 48 hours. Let me know if you have questions. — [Agent Name], [Brokerage]"

**To HOA — Request Documents Package:**
> "Hello, I'm the listing agent for [address]. Seller is [Name]. I'm requesting the full HOA document package: CC&Rs, current budget, reserve study, recent meeting minutes (last 12 months), and notice of any pending special assessments. Please confirm receipt and expected turnaround. My deadline is [date]. — [Agent Name], [Brokerage], [email]"

**To Buyer's Agent — Disclosure Delivery:**
> "Hi [Agent Name], attached is the complete disclosure package for [address], including: [list documents]. Please confirm receipt and have your buyer sign the acknowledgment page and return it within [timeframe] per the contract. — [Agent Name], [Brokerage]"

---

## STEP 6: ON-DEMAND COMPLIANCE AUDIT

When the agent says "run a compliance audit" or "are we good on disclosures?", generate:

**Compliance Audit Report — [Property Address]**

| Document | Required | Status | Days Since Contract | Risk Level |
|---|---|---|---|---|
| SPDS | Yes | Signed by seller | 2 days | 🟢 GREEN |
| Agency Disclosure | Yes | Delivered | Pre-contract | 🟢 GREEN |
| Lead Paint (pre-1978) | Yes | NOT DELIVERED | 2 days | 🔴 RED |
| HOA Documents | Yes | In progress | 2 days | 🟡 YELLOW |

**Outstanding Items + Recommended Actions** listed with urgency.

---

## MEMORY FORMAT

Each listing at `brain/disclosures/[address-slug].md`:

```markdown
# Disclosure Tracker: [Address]
Agent: [Name] | [Brokerage]
State: [State]
Property Type: [type]
Year Built: [year]
HOA: [Yes/No]
Contract Date: [YYYY-MM-DD]
Broker Contact: [name, contact]

## Documents
| Document | Required | Status | Due Date | Delivered | Acknowledged |
|---|---|---|---|---|---|
| SPDS | Yes | Signed by seller | [date] | [date] | [date] |
| Lead Paint | [Yes/No] | [status] | [date] | [date] | [date] |
| HOA Package | [Yes/No] | [status] | [date] | [date] | [date] |

## Notes
[Any special circumstances or seller-disclosed defects]

## Last Updated
[YYYY-MM-DD]
```

---

## HOW TO USE

- **At listing signing:** "New listing at 456 Oak Ave, 1972 build, no HOA, Arizona — generate my disclosure checklist"
- **At contract acceptance:** "We just went under contract at Oak Ave — what's due and when?"
- **Mid-transaction:** "Run a compliance audit on Oak Ave"
- **When stuck:** "Draft a message to the HOA for Oak Ave documents"
- **Before closing:** "Are all disclosures signed and acknowledged for Oak Ave?"

Disclosure failures are among the top reasons agents face lawsuits and license complaints. Use this skill on every listing, every time.
