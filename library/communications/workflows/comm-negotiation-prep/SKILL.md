---
name: comm-negotiation-prep
description: >-
  Use when the user says '/negotiation-prep', 'help me prepare for a negotiation', 'prep my BATNA for [deal]', 'I'm negotiating [contract/salary/terms]', or needs a full strategy before any high-stakes conversation. Delivers BATNA analysis, leverage mapping, counterparty research, objection scripts, and a step-by-step conversation flow guide. Covers salary, vendor contracts, deal terms, pricing, partnerships, and real estate negotiations.
---

# COMM-Negotiation-Prep
## Negotiation Strategy & Preparation

---

## Overview

This skill builds a complete negotiation strategy — BATNA analysis, leverage identification, counterparty research, objection scripts, and a structured conversation flow — for any high-stakes negotiation: salary, vendor contracts, deal terms, pricing, partnerships, or legal settlements.

**No regulated disclaimer required for this skill. Note: For legal or financial negotiations with material consequences, consult a licensed attorney or advisor before executing any agreement.**

---

## Pre-Flight — Client Preferences

1. Search Cloud Brain: `search_notes` with query "comm preferences"
2. If found: confirm in ONE line — "Prepping as [Name] at [Business], correct?"
   - If yes: proceed immediately
   - If no: ask what changed, update, save, proceed
3. If not found: collect setup in ONE message:
   - Your name, role, and business
   - Your default negotiation style (collaborative / competitive / principled / depends on situation)
   - Industries or deal types you negotiate most often
4. Save to `brain/preferences/comm-preferences.md`
5. Show a one-line preferences banner at top of output

---

## Job Inputs

Ask at the start of each run:
- What are you negotiating? (contract, salary, vendor price, deal terms, partnership, real estate, other)
- Who is the counterparty? (name, company, role — as much as known)
- What is your ideal outcome? What would you walk away happy with?
- What is your walk-away point? (the worst you'd accept before walking)
- What do you know about their situation, constraints, or motivations?
- What is your timeline? Any deadline pressures on either side?
- What's your current relationship with this person/company?

Do not save these answers to Cloud Brain.

---

## Output Structure

### 1. Situation Summary
Brief restatement of the negotiation context to confirm understanding before building strategy.

### 2. Your Position Analysis
**Ideal Outcome (Target):** [What you're aiming for]
**Acceptable Range:** [Min–max on key variables: price, terms, timeline, etc.]
**Walk-Away Point (BATNA):** [What you'll do if no deal is reached — be specific]
**BATNA Strength:** 🟢 Strong / 🟡 Moderate / 🔴 Weak — [brief rationale]

A strong BATNA gives you leverage. A weak one means you need to create it — identify alternatives before the negotiation.

### 3. Counterparty Analysis
**Their Likely Goals:** [What they want from this negotiation]
**Their Likely Constraints:** [Budget, timeline, authority limits, political factors]
**Their BATNA:** [What happens for them if no deal — the stronger their BATNA, the less pressure on them]
**Pressure Points:** [What creates urgency or discomfort for them]
**Decision Maker:** [Is the person at the table actually able to say yes?]

### 4. Leverage Map
| Source of Leverage | Yours | Theirs | Notes |
|---|---|---|---|
| Alternatives (BATNA strength) | [H/M/L] | [H/M/L] | |
| Timeline pressure | [H/M/L] | [H/M/L] | |
| Information advantage | [H/M/L] | [H/M/L] | |
| Relationship / goodwill | [H/M/L] | [H/M/L] | |
| Market / competitive position | [H/M/L] | [H/M/L] | |

**Overall leverage balance:** [Yours is stronger / theirs is stronger / roughly equal]

### 5. Opening Strategy
**Anchor first or wait?** [Recommendation with rationale]
**Your opening position:** [What to ask for — should be beyond your target to leave room]
**Framing:** [How to position your ask to make it feel reasonable]
**First question to ask them:** [The single best question to open with to gain information]

### 6. Objection Scripts
For each likely pushback:
```
OBJECTION: "[what they'll likely say]"
ACKNOWLEDGE: "[validate without conceding]"
REDIRECT: "[pivot back to your position or to their interest]"
CONCESSION OFFER (if needed): "[what you'd offer to move past this objection]"
```

### 7. Concession Strategy
- **Lead concessions:** [What you're willing to give up first — choose low-cost-to-you, high-value-to-them items]
- **Non-negotiables:** [What you will not concede under any circumstances]
- **Linkage plays:** [If you concede X, ask for Y in return — never give something without getting something]
- **Flinch language:** [How to react when they make a first offer that's outside your range]

### 8. Conversation Flow Guide
```
OPEN: [How to start — rapport, framing, agenda-setting]
EXPLORE: [Questions to ask to understand their position before revealing yours]
PROPOSE: [When and how to make your first offer]
TRADE: [How to handle the give-and-take phase]
CLOSE: [How to move toward agreement — what language to use]
WALKAWAY: [Exactly what to say if you need to exit without a deal]
```

### 9. Post-Negotiation Checklist
- [ ] Get any agreement in writing immediately
- [ ] Summarize agreed terms in an email within 24 hours
- [ ] Note what you learned about their constraints for future negotiations
- [ ] Assess: did you leave value on the table? What would you do differently?

---

## Memory

For significant negotiations, offer to save the prep strategy:

**Path:** `brain/communications/meetings/negotiation-prep-[slug]-[YYYY-MM-DD].md`

Ask: "Want me to save this strategy to your brain? Useful to have on hand during the negotiation."

---

## Error Handling

- **Missing counterparty info:** Build with best assumptions, flag gaps: "⚠️ [Item] unknown — research this before the meeting"
- **User's BATNA is weak:** Flag it directly: "🚨 Your BATNA is weak — before negotiating, identify at least one alternative. Going in without one is high risk."
- **No walk-away point given:** Ask for it — proceeding without a defined walk-away leads to over-concession
- **Legal contract negotiation:** Add flag: "⚠️ This involves a legal agreement — have a licensed attorney review final terms before signing"
