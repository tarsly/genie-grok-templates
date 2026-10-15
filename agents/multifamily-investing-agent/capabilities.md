# Multifamily Tools — Agent Capabilities Manifest

*For use by system-03 (Agent Designer) and system-04 (Agent Activator)*
*Last updated: 2026-06-12*

---

## About This Plugin

`multifamily-investing` is a two-phase apartment deal analysis system. Phase 0 (mf-screener) evaluates any multifamily deal in minutes and produces an LOI-ready output. Phase 1 (mf-underwriter) runs post-contract deep due diligence to verify every number before contingencies are released. Both skills share a single investor preferences profile so the client's buy box, DSCR minimums, and cash flow thresholds are pre-loaded from the first run and carry through every deal.

**Target users:** Multifamily investors acquiring apartment buildings — whether they're evaluating broker OMs on the fly, under contract and verifying financials, or running a disciplined screening process across multiple deals.

**Plugin version:** 1.0.1

---

## Available Skills Catalog

---

### 1. mf-screener

**What it does:** Pre-LOI quick screener for any multifamily deal. Takes a broker OM, T12, rent roll, or verbal summary and produces a full acquisition analysis: rent roll reality check, conservative NOI, 9-cell cap rate matrix, creative finance capital stack (assumption + seller carry + MLO + PML), true cash flow, three offer scenarios, viability score (1–10), red flag audit, pursuit decision, and a draft LOI. Saves screener output to cloud brain for automatic use by mf-underwriter.

**Preferences collected at activation:**
- Target markets or states
- Asset class preference (A / B / C / any)
- Unit count range (min/max)
- Deal size range — min/max purchase price
- Minimum monthly cash flow after all debt service
- Target cap rate range (e.g., 7–9%)
- Minimum acceptable DSCR
- Financing approach preference (creative-first vs. conventional-first)
- Risk tolerance (conservative / moderate / aggressive)

**Suggested schedule:** On-demand — triggered when a broker sends a deal or the client wants a fast read before submitting an LOI.

**Natural pairings:** mf-underwriter (screener output saved to brain feeds directly into full underwriting when the LOI is accepted).

---

### 2. mf-underwriter

**What it does:** Post-contract deep due diligence. Verifies all financials against T12, bank statements, and Schedule E actuals. Produces three-scenario NOI (Floor / Base / Upside), full valuation matrix, DSCR analysis at multiple price points, a formal Risk Matrix across four categories (Financial, Legal/Regulatory, Physical, Market), a 30-item due diligence checklist, 25+ hard seller questions, and a gated Go/No-Go verdict. Reads screener output from cloud brain as the starting baseline.

**Preferences collected at activation:** Same set as mf-screener — no separate interview needed if screener preferences are already saved.

**Suggested schedule:** On-demand — triggered when an LOI is accepted and the investor is under contract.

**Natural pairings:** mf-screener (screener output pre-loads as the baseline; always run screener first).

---

## Preferences Registry

Both skills read from and write to one shared path:
`brain/preferences/multifamily-preferences.md`

system-04 should collect all fields in a single interview — not once per skill.

| Field | Used By |
|---|---|
| Target markets / states | Both skills |
| Asset class preference (A / B / C / any) | Both skills |
| Unit count range (min/max) | Both skills |
| Deal size range — min/max purchase price | Both skills |
| Minimum monthly cash flow after all debt service | Both skills |
| Target cap rate range | Both skills |
| Minimum acceptable DSCR | Both skills |
| Financing approach preference (creative-first / conventional-first) | Both skills |
| Risk tolerance (conservative / moderate / aggressive) | Both skills |

---

## Suggested Agent Configuration

A single "Multifamily Analyst" agent handles both skills. There is no meaningful split — the screener and underwriter are sequential phases of the same workflow.

**Agent name suggestion:** Multifamily Analyst  
**Skills:** mf-screener, mf-underwriter  
**Activation interview:** All 9 fields from the Preferences Registry. Takes approximately 3–5 minutes.

---

## Recommended Schedules

| Skill | Schedule | Notes |
|---|---|---|
| mf-screener | On-demand | Triggered when a new deal arrives for evaluation. |
| mf-underwriter | On-demand | Triggered when an LOI is accepted and the investor goes under contract. |

Neither skill has a recurring cadence. Both are deal-triggered.

---

## How system-03 Should Use This Document

When a client indicates they invest in apartments or multifamily:

1. Surface the two-phase structure: screener for deal flow, underwriter for contracts. The client may not realize the handoff is automatic.
2. Assign both skills to one agent — there is no reason to split them.
3. Note that both skills are on-demand; no daily scheduling is needed unless the client specifically requests a deal pipeline report.

---

## How system-04 Should Use This Document

When activating the Multifamily Analyst agent:

1. Check cloud brain for existing preferences: `search_notes` with query `"multifamily preferences"`.
2. If preferences exist, confirm and proceed. If not, run the 9-question setup interview.
3. Save all preferences to: `brain/preferences/multifamily-preferences.md`
4. Both skills share this path — collect the fields once.
5. Schedule: both skills are on-demand. Confirm the client understands how to trigger each skill and when the screener-to-underwriter handoff occurs.
