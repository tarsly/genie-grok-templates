# RV Park Tools — Agent Capabilities Manifest

*For use by system-03 (Agent Designer) and system-04 (Agent Activator)*
*Last updated: 2026-06-12*

---

## About This Plugin

`rv-park-investing` is a two-phase acquisition toolkit for RV park and outdoor hospitality investors. Phase 0 (screener) gets an investor under contract smart and fast. Phase 1 (underwriter) verifies every assumption before contingencies are released. Preferences saved in Phase 0 carry forward automatically into Phase 1 — run them sequentially for any deal.

**Target users:** RV park investors, campground buyers, outdoor hospitality operators — any investor evaluating or underwriting an RV park or campground acquisition.

**Plugin version:** 1.0.1 (2 skills)

---

## Available Skills Catalog

---

### 1. rv-park-screener

**What it does:** Quick pre-LOI analysis using a T12, P&L, rent roll, or broker memo. Produces a NOI estimate, creative finance stack (Gator EMD + PML + seller carry + SBA assumption), true cash flow after debt service, 2–3 offer scenarios, and a draft LOI. Designed to get an offer in on the same day the broker sends the package.

**Preferences collected at activation:**
- Target markets / states
- Deal size range (min/max purchase price)
- Minimum monthly cash flow after all debt service
- Target cap rate range (e.g., 9–11%)
- Minimum acceptable DSCR
- Preferred financing approach (creative first vs. conventional)
- Risk tolerance (conservative / moderate / aggressive)

**Suggested schedule:** On-demand — triggered whenever a broker sends deal financials.

**Natural pairings:** rv-park-underwriter (always — screener output is read automatically by the underwriter).

---

### 2. rv-park-underwriter

**What it does:** Comprehensive post-contract due diligence. Verifies all screener estimates against bank statements and actuals. Runs 3-scenario NOI (floor/base/upside), a full valuation matrix, DSCR at every offer price, a formal Risk Matrix across 4 categories, a 28-item DD checklist, 25+ hard seller questions, and produces a gated Go/No-Go verdict.

**Preferences collected at activation:** Same fields as screener — loaded from memory automatically. No re-collection needed if screener was run first.

**Suggested schedule:** On-demand — triggered when an LOI is accepted and the investor goes under contract.

**Natural pairings:** rv-park-screener (always run screener first; underwriter reads screener output from memory as its baseline).

---

## Preferences Registry

All preferences stored at: `brain/preferences/rv-park-preferences.md`

| Field | Description |
|---|---|
| Target markets | States or regions the investor is actively buying in |
| Deal size range | Min/max purchase price the investor will consider |
| Min monthly cash flow | Minimum acceptable cash flow after all debt service |
| Target cap rate range | e.g., 9–11% — drives offer price recommendations |
| Min DSCR | e.g., 1.25x — gates Go/No-Go verdicts |
| Financing approach | Creative-first (assumption + seller carry) vs. conventional |
| Risk tolerance | Conservative / moderate / aggressive — calibrates verdict thresholds |

---

## Suggested Agent Configurations

---

### Option A: Solo Acquisition Agent (Recommended)

One "RV Park Analyst" agent handles both skills end-to-end.

**Best for:** Any investor acquiring RV parks. Run screener on every deal that comes in; trigger underwriter on anything that gets under contract.

**Activation interview:** All 7 preference fields above (~5 minutes). Saved once, applied to every deal.

---

### Option B: Two-Agent Setup

**Deal Screener Agent** — rv-park-screener only. Focuses on fast deal triage and LOI generation.

**Due Diligence Agent** — rv-park-underwriter only. Handles post-contract verification and Go/No-Go analysis.

**Best for:** Teams where deal sourcing and due diligence are handled by different people. Both agents share the same preferences memory path.

---

## Recommended Schedules

| Skill | Recommended Schedule | Notes |
|---|---|---|
| rv-park-screener | On-demand | Triggered by incoming broker packages. No recurring schedule needed. |
| rv-park-underwriter | On-demand | Triggered when LOI is accepted. Optionally set a reminder 5 days after screener runs: "Are you under contract yet?" |

---

## How system-03 Should Use This Document

1. Suggest this plugin for any client who identifies as an RV park or campground investor, or mentions outdoor hospitality acquisitions.
2. Both skills belong to the same agent in most configurations — they are a two-phase sequence, not independent tools.
3. Preferences are set once at activation and apply to every deal the investor runs — make this clear to the client.

## How system-04 Should Use This Document

1. Collect all 7 preference fields in a single setup interview (~5 minutes). Save to `brain/preferences/rv-park-preferences.md`.
2. Do not schedule recurring runs. Both skills are on-demand, triggered by deal flow.
3. Optionally schedule a lightweight prompt 5 days after any screener run: "Did this deal get under contract? If so, run rv-park-underwriter to begin full due diligence."
