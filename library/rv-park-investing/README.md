# RV Park Tools Plugin

A two-skill acquisition toolkit for RV park investors — from first look at a broker email to a Go/No-Go verdict before releasing contingencies.

## What's New in v1.0

- **Client preferences saved to memory** — your buy box (target markets, cap rate, min DSCR, min cash flow, financing approach) is captured once and applied to every analysis automatically
- **Screener now saves to memory** — the Phase 0 screener output is stored so the underwriter picks up where it left off, without re-entering deal details
- **All thresholds calibrated to your criteria** — offer scenarios, pursuit decisions, and Go/No-Go verdicts reference your stated minimums, not generic benchmarks

## Skills

### RV Park Screener (Phase 0 — Pre-LOI)

Fast deal analysis when a broker sends financials and you need an answer today. Give it a T12, P&L, rent roll, or a verbal summary and it produces:

- Client preferences check (reads from memory or runs a 60-second setup)
- Conservative NOI estimate with mandatory expense normalizations
- 9-cell valuation matrix anchored to the NOI floor, calibrated to your target cap rate
- Full 5-layer creative finance stack (Gator EMD + SBA assumption + seller carry + PML)
- True cash flow after all debt service with DSCR vs. your stated minimums
- 3 offer scenarios with your preferred financing approach as Offer 1
- 12-point red flag check including buy box fit
- Complete draft LOI ready to send
- Saves screener summary to memory for the underwriter

**Trigger phrases:** "screen this RV park," "quick look at this T12," "should I send an LOI," "what should I offer on this park," "is this worth pursuing," "build me an LOI," "run a quick deal analysis"

---

### RV Park Underwriter (Phase 1 — Post-Contract)

Comprehensive due diligence once you're under contract. Reads your preferences AND the screener's saved analysis automatically, then verifies every assumption before contingencies are released.

- Loads client preferences + screener findings from memory
- 7-step revenue verification against bank statements
- Full expense normalization with all mandatory add-backs
- Three-scenario NOI (Floor / Base / Upside)
- Full valuation matrix with client's target cap rate highlighted
- DSCR at contract, recommended, and floor prices vs. your minimum
- Risk Matrix across 4 categories (Financial, Legal/Regulatory, Physical/Infrastructure, Market)
- 28-item prioritized due diligence checklist
- 25 hard seller questions tailored to the deal
- Gated Go/No-Go verdict (GREEN / YELLOW / RED) calibrated to your risk tolerance

**Trigger phrases:** "underwrite this RV park," "run full RV park due diligence," "verify this deal," "go deep on this park," "we're under contract — analyze everything," "RV park DD"

---

## Two-Phase System

**Phase 0 — Screener** → Get under contract smart and fast. Saves deal summary to memory.
**Phase 1 — Underwriter** → Reads screener output, verifies everything, delivers verdict before you go hard.

## Updating Your Preferences

Run either skill and say "update my preferences" at any time. The skill will display your current settings and let you change any field. Changes are saved to memory immediately.


## Grok Bot import
See the root README. Replace `{{BOT_NAME}}` in `system-prompt.md` and `workflows/getting-started/SKILL.md`.
