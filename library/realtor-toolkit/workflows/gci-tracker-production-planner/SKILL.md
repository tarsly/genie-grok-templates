---
name: gci-tracker-production-planner
description: >-
  Annual GCI goal setting and production math for real estate agents — reverse- engineer your income goal into the exact number of leads, appointments, contracts, and closes you need each month. Track closed transactions and running GCI against goal. Monthly progress reports. Use at the start of any year or quarter, when setting production goals, or to check progress against targets. Triggers on: GCI goal, production plan, income goal, how many deals do I need, transaction tracker, annual goal, production tracker, commission tracker, how am I doing this year, deals closed, yearly goal.
---

# GCI Tracker & Production Planner

> **Disclaimer:** GCI projections and pipeline math are estimates based on assumed conversion rates and average commission amounts. Actual income will vary based on market conditions, transaction volume, negotiated commission rates, and brokerage splits. This skill is a professional planning tool — not financial advice. Consult a financial advisor or CPA for income planning, tax strategy, or business financial decisions.

You're building and tracking your **annual production plan** — the most important business tool a real estate agent has. The agents who consistently hit their income goals aren't luckier; they know their numbers and work backward from them every single month.

---

## STEP 0: PREFERENCES

Check memory for saved agent preferences first.

```
mcp__cloud-brain__search_notes: "realtor-toolkit preferences"
```

**If preferences found:** Display and confirm — "Using: [Agent Name], [Brokerage], [Market]. Commission rate: [X%]. Split: [X%]. Avg price: $[X]. Proceed or update?"

**If not found:** Run the setup interview below, then save.

> To build your production plan, I need a few business details:
> 1. Your full name?
> 2. Brokerage name?
> 3. Primary market / city?
> 4. Your typical listing commission rate? (e.g., 2.5%, 3%)
> 5. Your brokerage split — what percentage do YOU keep? (e.g., 70%, 80%, 100%)
> 6. Average sale price in your market? (rough estimate is fine)
> 7. Do you primarily work buyer-side, seller-side, or both?

```
mcp__cloud-brain__write_note:
  path: brain/preferences/realtor-toolkit-preferences.md
  content: |
    # Real Estate Agent Pro — Preferences
    Agent: [name]
    Brokerage: [brokerage]
    Market: [market]
    Commission Rate: [X%]
    Brokerage Split (agent keeps): [X%]
    Average Sale Price: $[X]
    Primary Business Focus: [buyer-side / seller-side / both]
    Updated: [date]
```

Show preferences banner at the top of every output:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Agent: [Name] | [Brokerage] | [Market]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## STEP 1: ANNUAL GOAL SETTING

### Step 1A: Income Goal Input

Ask:
- "What is your GCI goal for this year?" (Gross Commission Income — before brokerage split)
  - OR: "What do you want to take home after your split?"
- What year is this plan for?
- Current month (to calculate months remaining)

If agent gives a take-home number, back-calculate GCI:
> Take-home goal ÷ brokerage split % = GCI Goal
> Example: $120,000 take-home ÷ 80% split = $150,000 GCI goal

---

### Step 1B: Pipeline Math

Calculate backward from the GCI goal. Show all work so the agent can adjust assumptions.

```
PRODUCTION MATH
─────────────────────────────────────────────────
GCI Goal:                          $[goal]
Avg Commission Per Transaction:    $[avg price × commission rate]
  (Avg Sale Price: $[X] × [rate]% = $[X])
Transactions Needed (full year):   [GCI ÷ avg commission = X]
Months Remaining This Year:        [X months]
Transactions Per Month Needed:     [total ÷ months = X/month]
─────────────────────────────────────────────────
```

Apply pipeline conversion rates. Display the assumed rates and invite the agent to adjust if they know their own numbers:

```
PIPELINE CONVERSION ASSUMPTIONS
(Adjust these if you track your own conversion rates)
─────────────────────────────────────────────────
Leads → Appointments:        10% (10 leads = 1 appt)
Appointments → Signed:       50% (2 appts = 1 signed client)
Signed → Closed:             75% (1.33 signed = 1 close)
─────────────────────────────────────────────────
```

Calculate monthly pipeline targets:

```
MONTHLY PIPELINE TARGETS
─────────────────────────────────────────────────
Closes Needed/Month:           [X]
Signed Clients Needed/Month:   [X closes × 1.33 = X]
Appointments Needed/Month:     [signed × 2 = X]
Leads Needed/Month:            [appointments × 10 = X]
─────────────────────────────────────────────────
```

**Display as the full production dashboard:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Agent Name] | [Brokerage] | [Year] PRODUCTION PLAN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

ANNUAL GOAL
  GCI Goal:                    $[X]
  Take-Home (after split):     $[X] (at [split]%)
  Avg Commission/Deal:         $[X]
  Transactions Needed:         [X] deals

MONTHLY TARGETS ([X] months remaining)
  Leads Needed:                [X]/month
  Appointments Needed:         [X]/month
  Signed Clients Needed:       [X]/month
  Closes Needed:               [X]/month

LEAD SOURCE BREAKDOWN ([X] leads/month)
  Sphere/Referrals   (20%):    [X] leads/month
  Farming            (15%):    [X] leads/month
  Online Leads       (30%):    [X] leads/month
  Open Houses        (15%):    [X] leads/month
  Active Prospecting (20%):    [X] leads/month

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Save the plan to memory:
```
mcp__cloud-brain__write_note:
  path: brain/projects/gci-tracker-[year].md
  content: |
    # GCI Tracker — [Agent Name] — [Year]
    Goal: $[X] GCI / $[X] take-home
    Transactions Needed: [X]
    Monthly Targets: [X] leads, [X] appts, [X] signed, [X] closes
    Avg Commission/Deal: $[X]
    
    ## Closed Transactions
    [empty — populated as deals close]
    
    ## YTD Summary
    Transactions Closed: 0
    YTD GCI: $0
    Last Updated: [date]
```

---

## STEP 2: LOG A CLOSED TRANSACTION

When agent says "I just closed [address]" or "Log a deal":

Collect:
- Property address
- Sale price
- Agent's role: listing side / buyer side / both sides
- Close date
- Commission earned (calculate: sale price × commission rate × brokerage split — or agent can input the net figure directly)

Pull existing tracker from memory:
```
mcp__cloud-brain__search_notes: "gci-tracker-[year]"
```

Add the transaction and update running totals:
```
mcp__cloud-brain__edit_note:
  path: brain/projects/gci-tracker-[year].md
  [append transaction, update YTD counts and GCI total, update pace calculation]
```

Confirm immediately with a mini progress update:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Deal Logged: [Address] — [close date]
Commission: $[X]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
YTD: [X] deals | $[X] GCI | [X]% to goal
Pace: [on track / X deals ahead / X deals behind]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## STEP 3: MONTHLY PROGRESS REPORT

On command: "Show me my production report" or "How am I doing this year?"

Pull tracker from memory:
```
mcp__cloud-brain__search_notes: "gci-tracker-[year]"
```

Generate the full report:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Agent Name] | [Year] Production — [Month] Update
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

GOAL PROGRESS
  Annual GCI Goal:             $[X]
  YTD GCI:                     $[X] ([X]% to goal)
  Transactions Goal:           [X]
  YTD Transactions Closed:     [X] ([X]% to goal)

PACE ANALYSIS
  Months Elapsed:              [X] of 12
  Expected GCI at This Point:  $[X] (on-pace benchmark)
  Actual YTD GCI:              $[X]
  Variance:                    $[+/-X] ([ahead / behind] pace)
  Projected Year-End GCI:      $[X] (at current pace)

MONTHLY AVERAGES
  Goal (per month):            [X] deals | $[X] GCI
  Actual (per month YTD):      [X] deals | $[X] GCI

CLOSED TRANSACTIONS
  [MM/DD] | [Address]          | $[price] | [role] | Commission: $[X]
  [MM/DD] | [Address]          | $[price] | [role] | Commission: $[X]
  ...
  TOTAL: [X] transactions | $[X] GCI

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Pace interpretation line (always include one of these):**
- On track: "You're on pace to hit $[projected YTD] by year-end — [X] deals ahead of schedule. Keep your pipeline full and you'll exceed your goal."
- Ahead: "You're $[X] ahead of pace — on track to finish the year at approximately $[projected]. Outstanding."
- Behind: "You're $[X] behind pace. To hit your goal, see the recalibration below."

---

## STEP 4: GOAL RECALIBRATION

Triggered automatically when agent is behind pace, or on command: "Recalculate my targets."

```
RECALIBRATION — [Month]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
You're $[X] behind pace with [X] months remaining.

To hit your original goal of $[X] GCI:
  Remaining GCI Needed:        $[X]
  Transactions Remaining:      [X] deals in [X] months
  New Closes/Month Target:     [X]/month (was [X]/month)

Updated Monthly Pipeline Targets:
  Leads Needed/Month:          [X] (was [X])
  Appointments Needed/Month:   [X] (was [X])
  Signed Clients Needed/Month: [X] (was [X])
  Closes Needed/Month:         [X] (was [X])

OPTIONS:
A) Push to original goal — increase activity as shown above
B) Adjust goal to $[realistic adjusted goal] — maintains current pace
C) Identify your highest-leverage lead source and double down there

Which approach do you want to take?
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

After recalibration, update memory:
```
mcp__cloud-brain__edit_note:
  path: brain/projects/gci-tracker-[year].md
  [update: Last Recalibration: [date], Updated Monthly Target: [X], Notes: [option chosen]]
```
