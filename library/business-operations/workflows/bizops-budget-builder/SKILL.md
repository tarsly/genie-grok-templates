---
name: bizops-budget-builder
description: >-
  Build and track a business budget — create annual or quarterly budgets, log actuals vs. plan, analyze variances, flag overspend, and get recommendations to improve financial performance, or needs to set up a budget, log actuals against plan, or identify where spend is off track.
---

# Budget Builder — Plan vs. Actuals Tracker

## Overview

> **Disclaimer:** This skill produces financial planning analysis for informational purposes only. It is not a substitute for advice from a licensed financial advisor or CPA. Verify all figures with qualified professionals before making major financial decisions.

Budget Builder helps business owners create a budget, track actuals against it, and understand where they are on track vs. off track. Most small business owners have no formal budget — they react to money rather than plan it. This skill fixes that. Build your budget once, update actuals monthly, and get variance analysis that tells you exactly where your plan is working and where it is breaking down.

Works in three modes:
- **Setup:** Build a new annual or quarterly budget from scratch
- **Update:** Log actual revenue and expenses against the plan
- **Report:** Full variance analysis with flags and recommendations

## When This Skill Applies

- User says "build a budget" or "create a budget" or "set up a budget"
- User says "track my spending" or "actual vs. budget" or "budget vs. actuals"
- User says "am I over budget?" or "where am I off track?"
- User says "update my budget" or "log this month's numbers"
- User says "variance report" or "budget report"
- User says "how much have I spent on [category]?"
- User says "budget for next year" or "plan for next quarter"
- User says "what's my budget for [category]?"
- User asks if they can afford something — pull the budget to contextualize

## Pre-Flight — Budget Preferences

1. Search Cloud Brain: `search_notes` with query `"bizops budget preferences"` (folder: `brain/preferences`)
2. **If found:** Display as a table, confirm, then proceed
3. **If not found:** Ask in ONE message:
   - Business name and industry
   - Budget period (annual / quarterly — quarterly recommended for most small businesses)
   - Fiscal year start month
   - Primary currency
   - Major expense categories relevant to your business (e.g., payroll, rent, software, marketing, cost of goods, contractors)
   - Save to Cloud Brain: `write_note` → title: `bizops-budget-preferences`, folder: `brain/preferences`
4. Apply throughout: use saved categories for all budget line items, fiscal period for date calculations
5. Show banner at top of every output:
   ```
   💰 Budget Builder | {Business Name} | {Period} | Preferences: ✓ loaded
   ```
   Add: *"Say 'update my budget preferences' to change settings."*

## How It Works

### Mode 1: Budget Setup

When the user is creating a budget for the first time (or a new period):

**Step 1: Determine the period**
- Ask: "Are we building an annual or quarterly budget? What period are we planning for?"
- Use preferences for fiscal year start if saved

**Step 2: Collect revenue plan**
For each revenue stream the user identifies:

| Revenue Stream | Monthly Target | Annual Target | Notes |
|---------------|---------------|---------------|-------|
| {Stream 1} | ${X} | ${X×12} | |
| {Stream 2} | ${X} | ${X×12} | |
| **Total Revenue** | **${X}** | **${X}** | |

**Step 3: Collect expense plan**
Use saved categories from preferences, or collect them now. Group into:
- **Fixed costs** — same every month (rent, salaries, subscriptions, insurance)
- **Variable costs** — fluctuate with revenue (COGS, commissions, contractor fees)
- **One-time costs** — planned but non-recurring (equipment, events, projects)

| Expense Category | Type | Monthly Budget | Annual Budget |
|-----------------|------|---------------|---------------|
| {Category} | Fixed/Variable/One-time | ${X} | ${X} |

**Step 4: Calculate summary**

```
BUDGET SUMMARY — {Business Name} — {Period}
============================================
Total Revenue Target:   ${X}
Total Expenses Budget:  ${X}
-------------------------------------------
Net Profit Target:      ${X}
Target Profit Margin:   {X}%
```

**Step 5: Save to Cloud Brain**
Use `write_note`:
- title: `budget-{business-slug}-{period}-{YYYY}`
- folder: `brain/budgets`
- tags: `["budget", "financial", "active"]`

---

### Mode 2: Update Actuals

When the user is logging actual numbers for a period:

**Step 1: Load the budget**
Use `search_notes` with query `"budget {business name} {period}"` (folder: `brain/budgets`) to find the active budget. If not found, prompt to run Setup first.

**Step 2: Collect actuals**
Ask for actuals by category (or accept them in bulk if the user pastes data):

```
"What were your actual numbers for [period]? Give me revenue by stream and expenses by category."
```

**Step 3: Save updated budget with actuals**
Use `write_note` with `overwrite: true` to update the existing budget note with actuals column added.

---

### Mode 3: Variance Report

When the user wants to see how actuals compare to the plan:

**Step 1: Load budget note**
Use `search_notes` → `read_note` to retrieve the current budget with actuals.

**Step 2: Calculate variances**

For each line item:
- **Variance $** = Actual − Budget
- **Variance %** = (Actual − Budget) / Budget × 100
- **Flag:** 🚨 if >15% over budget on expenses OR >15% under on revenue; ⚠️ if 5–15%

**Step 3: Output the variance report**

```
BUDGET VARIANCE REPORT — {Business Name} — {Period}
===================================================

REVENUE
| Stream | Budget | Actual | Variance $ | Variance % | Status |
|--------|--------|--------|-----------|-----------|--------|
| {name} | ${X} | ${X} | +/- ${X} | +/- X% | ✅/⚠️/🚨 |

EXPENSES
| Category | Budget | Actual | Variance $ | Variance % | Status |
|----------|--------|--------|-----------|-----------|--------|
| {name} | ${X} | ${X} | +/- ${X} | +/- X% | ✅/⚠️/🚨 |

SUMMARY
| | Budget | Actual | Variance |
|---|--------|--------|---------|
| Total Revenue | ${X} | ${X} | +/- ${X} |
| Total Expenses | ${X} | ${X} | +/- ${X} |
| Net Profit | ${X} | ${X} | +/- ${X} |
| Profit Margin | X% | X% | +/- X pts |

WHAT'S OFF TRACK
🚨 [Category]: $X over budget — {brief explanation of why this matters}
⚠️ [Stream]: $X under revenue target — {brief explanation}

TOP 3 RECOMMENDATIONS
1. {Specific action to close the biggest gap}
2. {Specific action for second-biggest issue}
3. {Proactive suggestion for the remainder of the period}
```

## Output Formatting Rules

- Always show budget and actuals side by side — never one without the other
- Color coding: ✅ on track (within 5%), ⚠️ watch (5–15% off), 🚨 action needed (>15% off)
- Revenue variances: positive = good (over target), negative = bad (under target)
- Expense variances: negative = good (under budget), positive = bad (over budget)
- Always end with numbered recommendations — never just a table with no action

## Memory Paths

| Content | Path |
|---------|------|
| Preferences | `brain/preferences/bizops-budget-preferences.md` |
| Budget files | `brain/budgets/budget-{slug}-{period}-{YYYY}.md` |

## Error Handling

- **No budget found for the period:** "I don't have a budget on file for {period}. Say 'build a budget' to set one up first."
- **Partial actuals provided:** Build the variance report with what is available, label missing categories as "Actuals pending." Note: "Provide the remaining actuals for a complete picture."
- **User provides only totals (no breakdown):** Use totals for the summary and flag: "For category-level analysis, provide a breakdown of revenue streams and expense categories."
- **Multiple businesses:** If preferences show multiple businesses, ask which one this budget is for before proceeding.
- **Budget already exists for this period:** "A budget for {period} already exists. Do you want to update it with new actuals, or start a fresh budget for this period?"
