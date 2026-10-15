---
name: listing-launch-checklist
description: >-
  Generate a complete, personalized listing launch workflow from signed agreement to live on MLS. Ensures nothing gets dropped and every listing launches like a professional production. Use when taking any new listing, need a photography brief, staging checklist, MLS input sheet, pre-launch timeline, showing instructions, or disclosure tracker. Triggers on: new listing, listing checklist, taking a listing, listing launch, pre-listing, photography brief, MLS input, staging checklist, listing prep, going live on MLS.
---

> **Disclaimer:** This skill generates professional workflow materials for real estate agents. MLS rules, listing requirements, disclosure obligations, and photography/staging timelines vary by market and brokerage. Always verify your local MLS rules and brokerage policies before launching any listing. This skill is a productivity tool — not a substitute for your broker's review.

# Listing Launch Checklist

## STEP 0: PREFERENCES

Check memory for saved agent preferences first.

```
mcp__cloud-brain__search_notes: "realtor-toolkit preferences"
```

**If preferences found:** Display and confirm — "Using: [Agent Name], [Brokerage], [Market]. Proceed or update?"

**If not found:** Run the setup interview below, then save:

```
mcp__cloud-brain__write_note:
  path: brain/preferences/realtor-toolkit-preferences.md
  content: |
    # Real Estate Agent Pro — Preferences
    Agent: [name]
    Brokerage: [brokerage]
    Market: [market]
    MLS Platform: [mls-platform]
    Default Photographer: [photographer-name, phone]
    Default Stager: [stager-name, phone]
    Updated: [date]
```

**Setup interview questions:**
1. Your full name?
2. Brokerage name?
3. Primary market / city?
4. MLS platform you use (e.g., Flexmls, Matrix, Stellar MLS)?
5. Default photographer contact (name, phone) — optional?
6. Default stager contact (name, phone) — optional?

Show a preferences banner at the top of every output:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Agent: [Name] | [Brokerage] | [Market]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

You're taking a new listing. This skill generates your complete listing launch package — a professional workflow from signed agreement to live on MLS. Every detail covered, nothing slips.

## Step 1: Gather Listing Details

Collect the following information about the property. If you're providing property details directly, I'll parse them. Otherwise, I'll ask clarifying questions:

**Property Basics:**
- Full property address
- List price
- Target go-live date (when should it hit MLS?)
- Property type (single family, condo, townhome, land)

**Seller Information:**
- Seller name(s)
- Seller's timeline / motivation
- Any red flags or challenges

**Property Features:**
- Bedrooms / bathrooms
- Square footage
- Garage (0/1/2/3+, type)
- Lot size
- Year built
- Recent updates / renovations
- Unique selling points (views, location, pool, etc.)
- Concerns to address in marketing

Provide as much or as little detail as you have. I'll generate your full package from whatever you give me.

---

## Step 2: Your Complete Listing Launch Package

I'll generate all of the following, customized to your property:

### 1. PHOTOGRAPHY BRIEF
A detailed shot list for the photographer covering:
- Must-have exterior angles and details (front, back, sides, landscaping)
- Interior priority rooms (master, kitchen, main living areas)
- Detail shots (high-end finishes, fixtures, built-ins)
- Drone / twilight recommendations (if applicable)
- Staging notes to communicate to seller before shoot day
- Timeline and next steps

### 2. STAGING CHECKLIST
Room-by-room prep list for the seller:
- What to declutter in each space
- Deep cleaning priorities
- Furniture arrangement tips
- Curb appeal quick wins
- Major "don't do" list (no air fresheners, remove personal photos, pet odors, clutter, etc.)
- Timeline to complete before photography

### 3. MLS INPUT SHEET
Pre-filled template with all required MLS fields (auto-populated with agent name and brokerage from preferences):
- Compelling property description (250+ words)
- Feature checkboxes (fireplace, hardwood, appliances, etc.)
- Showing instructions and access info
- Disclosures checklist
- Open house dates (if applicable)
- Copy-paste ready for your MLS portal

### 4. PRE-LAUNCH TIMELINE
Reverse countdown from go-live date with tasks and owners:
- **T-14 days:** Sign listing agreement, order photography, schedule pre-inspection
- **T-10 days:** Photography shoot, gather disclosure documents
- **T-7 days:** Staging walkthrough with seller, final prep
- **T-5 days:** Review photos, write MLS description, prep marketing materials
- **T-3 days:** Load to MLS as "Coming Soon," send to buyer agent network
- **T-1 day:** Final review, confirm open house schedule
- **T-0:** Go live, post to social media, send to sphere

### 5. SHOWING INSTRUCTIONS DRAFT
Ready for your MLS showing instructions (auto-populated with agent name from preferences):
- Access type (lockbox, agent showing, keypad, etc.)
- Notice required (24/48/72 hours)
- Pet and alarm instructions
- Seller preferences (times to avoid, no showing stickers, etc.)
- Parking and entry notes
- Cancellation policy

### 6. DISCLOSURE TRACKER
Standard residential disclosures with status checkboxes:
- Seller's property disclosure
- Lead-based paint disclosure (if pre-1978)
- HOA documents (if applicable)
- Local transfer tax forms
- Property condition report
- Inspection results (once ordered)

---

## How to Use This Skill

1. Tell me: **"I'm taking a new listing at [address] for [price], go-live [date]"** — plus any other details you have.
2. I'll ask clarifying questions if needed to fill in gaps.
3. I'll generate your complete 6-part launch package ready to print, share, or customize.
4. Use it to brief your photographer, coach your seller, load your MLS, and track pre-launch tasks.

Keep this checklist in your CRM or project management tool. Tick off tasks as you move through the timeline. When you hit T-0, your listing launches clean and professional.

---

**Ready?** Share your listing details and I'll build your launch package now.
