---
name: tax-audit-prep
description: >-
  Audit-readiness checklist and scorecard. Whether you've received an IRS notice or just want to be ready, this skill produces a document checklist by audit type (correspondence audit, office audit, field audit, RE-specific exam, Schedule C audit), grades your current documentation, and flags weak spots. Outputs a binder-ready file index and a CPA hand-off package.
---

# Tax Audit Prep

> **NOT TAX ADVICE.** Audits are CPA / EA / tax attorney territory. This skill organizes documents and surfaces gaps so you walk into your CPA's office ready, not scrambling.

## Overview

Six modes (one per `--type`):
1. **Correspondence audit** — IRS sent a letter requesting documentation
2. **Office audit** — In-person at IRS office, specific issues
3. **Field audit** — Examiner visits your home/business; broadest scope
4. **RE-specific exam** — Common targets: RE Pro status, passive activity, 1031 exchange
5. **Schedule C audit** — Small biz expense review, home office, vehicle, meals
6. **Proactive** — No audit, just want to be ready

Each mode produces:
- A document checklist (what the IRS will likely ask for)
- A grading rubric (you have it / partial / missing)
- A gap-closure plan
- A CPA hand-off package

## When This Skill Applies

- User received an IRS notice (CP2000, CP504, CP14, LT-11, etc.) and asks "what do I do?"
- User is being audited and wants to prep
- User wants a proactive readiness check
- User mentions: IRS audit, examination, CP2000, audit letter, audit reconsideration, documentation request

## Pre-Flight — Preferences

Load `tax preferences`. Asks if the trigger is a real audit or proactive:
- Active audit? (yes — what type / no — proactive)
- If active: paste the IRS notice number (CP## or LT##) so we map to the right protocol

Banner:
```
🎯 Audit Prep | Type: {type} | Year: {year} | Status: {active/proactive}
```

## How It Works

### Step 1: Identify the Trigger

If user has an IRS notice, look up the notice code:

| Notice | What it means | Urgency |
|--------|--------------|---------|
| CP2000 | Income mismatch — usually 1099 vs return | Respond within 30 days |
| CP14 | Balance due | Pay or set payment plan |
| CP504 | Final notice — intent to levy state refund | Urgent — 30 days |
| LT11 / Letter 1058 | Final notice intent to levy | Very urgent — CDP rights |
| CP2501 | Income discrepancy precursor to CP2000 | 30 days |
| Letter 525 | 30-day letter | Respond or proceed to Appeals |
| Letter 3219 | 90-day letter — Statutory Notice of Deficiency | Petition Tax Court within 90 days |

Map to scope. CP2000 ≠ a full audit; it's a math/matching issue. Letter 3219 is the cliff before Tax Court.

### Step 2: Build the Document Checklist

By audit type:

**Schedule C audit:**
- All bank statements (business and personal — examiners may request both)
- Credit card statements for any deducted expense
- Receipts for ALL deductions claimed > $75 (mandatory: meals, travel, equipment)
- Mileage log (contemporaneous — not reconstructed after the fact)
- Home office: square footage measurement, photos, exclusive-use documentation
- Vehicle: log of personal vs. business miles, vehicle title, lease/loan docs
- Cell phone: log of business vs. personal use percentage
- Internet: same
- 1099-NECs received (income verification)
- Invoices issued (revenue verification)
- W-9s collected (for any 1099-NEC you issued)

**RE-specific exam:**
- Closing statements (HUD-1 or Settlement Statement) for every property
- Mortgage statements (annual escrow / interest summary)
- Property tax payment proof
- Insurance bills + payments
- Repair receipts (distinguish repair vs. improvement — improvements capitalize)
- Improvement receipts (separate file)
- Tenant ledger / rent received documentation
- Lease agreements
- 1099-MISC issued to contractors > $600
- Depreciation schedules
- If claiming RE Pro: time logs by activity (acquisition, management, repairs, ed) — must show 750+ hours AND >50% of personal service time in RE
- If 1031 exchange: QI agreement, identification letter (45 days), closing on replacement (180 days), exchange basis calculation

**Field audit (broad):**
- Everything from Schedule C + RE
- All business contracts
- Loan documents
- Foreign accounts (FBAR, Form 8938)
- Cryptocurrency: every transaction, basis, fair market value
- Cash deposits > $10K (Form 8300)
- Owner draws / shareholder loans (S-Corp/C-Corp) — paper trail

**Correspondence audit (focused):**
- Only what the letter asks for
- A cover letter referencing the notice number, year, and your taxpayer ID
- A response date <= letter deadline

### Step 3: Grade Each Document

For each document, mark:
- ✓ I have it, well-organized
- ◯ Partial — I have some, need to gather rest
- ✗ Missing — need to reconstruct or get from third party

Calculate readiness score: (✓ count × 2 + ◯ count) / (total × 2) × 100

### Step 4: Gap-Closure Plan

For each ✗ or ◯, write the closure action:
- Bank statements missing → contact bank, request 7 years of statements (banks must keep 5+ years)
- Receipts missing → reconstruct from credit card statements + memory + vendor records
- Mileage log missing → reconstruct from Google Timeline, calendar, customer addresses (contemporaneous logs are far stronger — note this)
- Improvement vs. repair confusion → categorize against IRS regs (improvement = adapts, betters, restores)
- RE Pro hours undocumented → reconstruct from calendar, email, contractor records — note this is HARD to win on reconstruction

### Step 5: CPA Hand-Off Package

Produce a CPA-ready summary:
- Audit type and notice number
- Year(s) under examination
- Inventory of documents (with file paths or links)
- Identified gaps and reconstruction plan
- Issues that need legal/tax expertise (don't try to answer yourself)
- Timeline (deadlines, hearings)
- Questions for CPA (specific, not "what do I do")

### Step 6: Save

- **title:** `Audit Prep — {type} — {year} — {YYYY-MM-DD}`
- **folder:** `brain/tax/audits`
- **tags:** `["audit-prep", "{type}", "{year}"]`

## Data Structure

```markdown
# Audit Prep — {Type} — Tax Year {Year}

> **Generated:** {YYYY-MM-DD}
> **Active Audit:** {yes/no}
> **Notice Number:** {code or "—"}
> **Examiner / IRS Contact:** {name + phone if known}
> **Response Deadline:** {YYYY-MM-DD}
> **Readiness Score:** {N}%

## Audit Scope

{1-2 sentences — what's being examined}

## Document Checklist

### Income Documentation
- [{✓ / ◯ / ✗}] All 1099s received — file: {path}
- [{✓ / ◯ / ✗}] Bank statements (business) — months: {list}
- [{✓ / ◯ / ✗}] Invoices issued — file: {path}
- [{✓ / ◯ / ✗}] {item}

### Expense Documentation
- [{✓ / ◯ / ✗}] {item}

### Schedule-Specific (Schedule C / E / etc.)
- [{✓ / ◯ / ✗}] {item}

### RE-Specific (if applicable)
- [{✓ / ◯ / ✗}] {item}

## Gaps and Closure Plan

| Gap | Severity | Closure Action | Owner | Deadline |
|-----|----------|---------------|-------|----------|
| {gap} | {high/med/low} | {action} | {you/CPA/bank/etc.} | {date} |
| ... |

## CPA Hand-Off Package

### Summary
{2-3 sentences for the CPA}

### Document Inventory
{Cloud Brain link to organized files}

### Identified Gaps
{list}

### Specific Questions for CPA
1. {question}
2. {question}

### Timeline
- Response due: {date}
- CPA review needed by: {date}
- {other deadlines}

## Soft Reminders

- DO NOT respond to the IRS without CPA / EA / tax attorney review.
- DO NOT volunteer information beyond what's asked.
- All correspondence should reference the notice number.
- Keep copies of everything you send. Use certified mail with return receipt for paper responses.

## Risk Assessment

- **Underpayment risk:** {low/med/high} — {reasoning}
- **Penalty exposure:** {accuracy / fraud / late / etc.}
- **Statute of limitations:** {3-year (normal) / 6-year (>25% omission) / unlimited (fraud)}

## Recommended Next Step

1. Forward this prep package to {CPA name} immediately
2. Schedule CPA call within {N} days
3. Begin gap-closure on {top 3 items}
4. Do NOT contact IRS until CPA reviews response

```

## Output Format (Chat)

```
🎯 AUDIT PREP — {type} — Year {year}
Notice: {code or none} • Deadline: {date}

READINESS SCORE: {N}%

DOCUMENTS
✓ Have ({N}):       {brief list}
◯ Partial ({N}):    {brief list}
✗ Missing ({N}):    {brief list}

TOP 3 GAPS (CLOSE THIS WEEK)
1. {gap} — {action}
2. {gap} — {action}
3. {gap} — {action}

CPA HAND-OFF READY: brain/tax/audits/Audit Prep — {type} — {year}

REMINDERS
- Don't contact IRS without CPA review
- Don't volunteer info beyond the ask
- Certified mail for paper response
```

## Example Usage

**User:** "I got a CP2000 for 2024 saying I underreported $18K. What do I do?"

**AI:** Type: correspondence. Notice: CP2000. Maps to income mismatch — almost always a missing/late 1099. Asks user to paste the notice. Checklists: prior 1099s received, bank deposits, contract records. Generates response letter template. Strongly directs to CPA before sending.

**User:** "/tax-audit-prep --type proactive --year 2025"

**AI:** Proactive readiness check on 2025. Pulls user's tax-preferences (Schedule C + RE). Generates full checklist. User grades each item. Outputs readiness score and gap plan.

**User:** "I'm an RE Pro and they're auditing my 2023 — challenging my hours"

**AI:** Type: re-exam. Highest-stakes audit category. Generates: time log requirements, contemporaneous documentation standard (not reconstructions), what counts as "material participation," what doesn't (passive investor activities). Strongly directs to a tax attorney experienced in §469 cases.

**User:** "Field audit notice — 2022, 2023, 2024. Schedule C consulting + 4 rentals."

**AI:** Broadest scope. Generates 50+ item checklist. Notes 3-year scope = standard, but examiner can expand if fraud indicators. Recommends CPA + tax attorney now. Generates urgent timeline (response in 30 days).

## Error Handling

- **If user has no CPA / EA / tax attorney:** Strongly recommend hiring one BEFORE responding. List of starting points: AICPA find-a-CPA, NAEA find-an-EA. Don't try to navigate the audit solo.
- **If notice is past the response deadline:** Note urgency — many IRS rights have hard time limits (CDP, Tax Court). Recommend immediate attorney engagement.
- **If user has unreported income they know about:** STOP. Recommend tax attorney immediately. Voluntary Disclosure Program may be relevant. Do NOT log details in Cloud Brain.
- **If user wants the skill to draft IRS correspondence:** Do not draft for transmission. Provide a CPA-ready outline. The CPA/EA/attorney drafts and signs.
- **If documentation can't be reconstructed:** Some deductions may need to be conceded. Note the cost of conceding vs. fighting (penalty + interest may be cheaper than fees).
- **If audit involves criminal exposure (fraud, abusive shelters, FBAR willfulness):** Tax attorney territory, not CPA. CPAs don't have attorney-client privilege; communications can be compelled.
- **If user has multiple years under examination:** Cross-reference findings — examiners look for patterns. A weakness in year 1 may propagate.
- **If user is RE Pro and hours are being challenged:** Reconstructed logs are weak evidence. Contemporaneous calendar, email, contractor invoices, MLS access logs, broker statements help. Note: courts are skeptical of reconstructions.

## See Also

- `/tax-deduction-finder` — surface miscategorized expenses before the audit (same plugin)
- `/tax-cost-segregation` — relevant if cost seg is being audited (same plugin)
- `/bizops-sop-builder` — document going-forward processes so the next audit is easier (from `business-operations`)
