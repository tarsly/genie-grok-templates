---
name: bizops-client-onboarding
description: >-
  Onboard new clients — create contact records, project files, welcome materials, onboarding checklists, and intake workflows, or needs to set up a contact record, project file, welcome message, or intake checklist for a new client.
---

# Client Onboarding

## Overview

A comprehensive new client onboarding workflow that takes basic client information and sets up everything needed for a successful working relationship. In one pass, it creates: a contact entry in the people directory, a project file for their work, a welcome email draft, an onboarding checklist, and an initial meeting agenda. Everything is cross-linked so the AI can find client context from any entry point.

## When This Skill Applies

- User signs a new client and needs to set everything up
- User says "onboard {client name}" or "new client: {name}"
- User asks to set up files for a new client
- User needs a welcome email drafted for a new client
- User wants to create an onboarding checklist
- User mentions client intake or client setup
- User says "I'm starting to work with {name}" in a client context

## Pre-Flight — Business Preferences

1. Search Cloud Brain: `search_notes` with query `"bizops client onboarding preferences"` (folder: `brain/preferences`)
2. **If found:** Display as a table, confirm, then proceed
3. **If not found:** Ask in ONE message:
   - Business name and industry
   - Type of work you do for clients (consulting / coaching / services / products / other)
   - Standard payment terms (Net 15 / Net 30 / Due on receipt)
   - Your preferred communication style with clients (formal / casual / email-first / text-first)
   - Save to Cloud Brain: `write_note` → title: `bizops-onboarding-preferences`, folder: `brain/preferences`
4. Apply throughout: use business name in welcome emails, service type for checklist customization, payment terms in project files
5. Show banner at top of every output:
   ```
   🤝 Client Onboarding | {Business Name} | {Service Type} | Preferences: ✓ loaded
   ```
   Add: *"Say 'update my onboarding preferences' to change settings."*

## How It Works

### Step 1: Gather Client Information

Extract from the user's message (ask for essentials if missing, skip non-essentials):

**Essential:**
| Field | Description |
|-------|-------------|
| Client Name | Full name of the primary contact |
| Company/Entity | Business name (if applicable) |
| Service/Engagement | What you are doing for them |

**Helpful but Optional:**
| Field | Description | Default |
|-------|-------------|---------|
| Email | Primary email | "TBD — ask during first call" |
| Phone | Primary phone | "TBD" |
| Address | Mailing/billing address | "TBD" |
| How They Found You | Referral, social, website, etc. | "Not specified" |
| Engagement Value | Dollar value of the deal/contract | "TBD" |
| Start Date | When the engagement begins | Today's date |
| Key Dates | Any important deadlines | None |
| Special Notes | Preferences, requirements, context | None |

### Step 2: Create Contact Entry

Use `mcp__cloud-brain__write_note` with:
- **title:** `{Full Name}`
- **folder:** `people`
- **tags:** `["person", "client", "active"]`
- **content:** The contact record in this structure:

```markdown
# {Full Name}

> **Role:** Client
> **Company:** {company}
> **Status:** Active
> **Since:** {YYYY-MM-DD}
> **Priority:** {High if active engagement}

## Contact Info

| Field | Value |
|-------|-------|
| Email | {email} |
| Phone | {phone} |
| Address | {address} |
| Preferred Contact | {email/phone/text} |

## Relationship Context

- **How they found us:** {source}
- **Engagement:** {what we are doing for them}
- **Value:** ${amount}
- **Start Date:** {date}

## Key Notes

- {any special notes, preferences, or context}

## Interaction History

| Date | Type | Summary |
|------|------|---------|
| {today} | Onboarding | Client onboarded. Files created. |

## Linked Notes

- Project: [[{Client Name} — {Engagement Type}]]
- Invoices: search Cloud Brain for "invoice {client-slug}"
```

### Step 3: Create Project File

Use `mcp__cloud-brain__write_note` with:
- **title:** `{Client Name} — {Engagement Type}`
- **folder:** `projects`
- **tags:** `["project", "client", "active"]`
- **content:**

```markdown
# {Client Name} — {Engagement Type}

> **Status:** In Progress
> **Priority:** High
> **Client:** [[{Full Name}]]
> **Start Date:** {YYYY-MM-DD}
> **Target Completion:** {date or "Ongoing"}
> **Value:** ${amount}

## Scope

{Description of what is being delivered, key deliverables, and boundaries}

## Milestones

| # | Milestone | Target Date | Status |
|---|-----------|-------------|--------|
| 1 | Onboarding complete | {date} | In Progress |
| 2 | {milestone} | {date} | Not Started |
| 3 | {milestone} | {date} | Not Started |
| 4 | Final delivery / Close | {date} | Not Started |

## Tasks

- [ ] Complete onboarding checklist
- [ ] Send welcome email
- [ ] Schedule kickoff call
- [ ] {task specific to engagement}
- [ ] {task specific to engagement}

## Notes

- {date}: Client onboarded. Engagement initiated.

## Blockers

None currently.
```

### Step 4: Generate Welcome Email Draft

Use `mcp__cloud-brain__write_note` with:
- **title:** `Welcome Email — {Client Name}`
- **folder:** `drafts`
- **tags:** `["draft", "email", "client-onboarding"]`
- **content:**

```markdown
# Welcome Email — {Client Name}

> **To:** {client email}
> **Subject:** Welcome! Here's what to expect working with us

---

Hi {First Name},

Welcome aboard! I'm excited to start working together on {brief description of engagement}.

Here's what happens next:

**1. Kickoff Call**
We'll schedule a {30/60}-minute call to align on goals, timeline, and expectations. I'll send a calendar invite shortly.

**2. Information I'll Need From You**
To get started, I'll need a few things:
{bulleted list of items needed — documents, access, info, etc. Customize based on engagement type}

**3. How We'll Communicate**
{Communication preferences — email for formal, text/call for urgent, weekly check-ins, etc.}

**4. Timeline**
{Brief overview of key dates and milestones}

If you have any questions in the meantime, don't hesitate to reach out. Looking forward to getting results.

Best,
{User's name}
{User's title/company}
{Contact info}
```

### Step 5: Generate Onboarding Checklist

Use `mcp__cloud-brain__write_note` with:
- **title:** `Onboarding Checklist — {Client Name}`
- **folder:** `projects`
- **tags:** `["checklist", "client-onboarding", "active"]`
- **content:**

```markdown
# Onboarding Checklist — {Client Name}

> **Created:** {date}
> **Target Completion:** {7-14 days from start}

## Pre-Kickoff

- [ ] Contact entry created in Cloud Brain (people folder)
- [ ] Project file created in Cloud Brain (projects folder)
- [ ] Welcome email sent
- [ ] Kickoff call scheduled
- [ ] Client info collected (email, phone, address)

## During Kickoff

- [ ] Goals and expectations aligned
- [ ] Timeline and milestones confirmed
- [ ] Communication preferences established
- [ ] Key contacts identified (if company has multiple stakeholders)
- [ ] Required documents/access requested

## Post-Kickoff

- [ ] Meeting notes filed
- [ ] Action items documented in project file
- [ ] First deliverable/milestone scheduled
- [ ] Follow-up email sent with meeting summary
- [ ] Invoice configuration set up (if applicable)

## Engagement-Specific Items

- [ ] {Custom item based on service type}
- [ ] {Custom item based on service type}
- [ ] {Custom item based on service type}

## Handoff to Active Work

- [ ] All onboarding items complete
- [ ] Client confirmed ready to proceed
- [ ] Project status updated to "Active"
- [ ] First milestone work initiated
```

### Step 6: Generate Initial Meeting Agenda

Use `mcp__cloud-brain__write_note` with:
- **title:** `Kickoff Agenda — {Client Name}`
- **folder:** `drafts`
- **tags:** `["draft", "agenda", "client-onboarding"]`
- **content:**

```markdown
# Kickoff Meeting Agenda — {Client Name}

> **Date:** {TBD — to be scheduled}
> **Duration:** 30-60 minutes
> **Attendees:** {User}, {Client Name}

---

## 1. Introductions and Context (5 min)
- Quick personal intros (if not already acquainted)
- How they found us / what prompted the engagement

## 2. Goals and Expectations (10 min)
- What does success look like for them?
- Primary goals for this engagement
- Any concerns or past experiences to be aware of

## 3. Scope and Deliverables (10 min)
- Confirm what is being delivered
- Clarify what is NOT included (boundaries)
- Walk through milestones and timeline

## 4. Process and Communication (10 min)
- How we will work together (tools, frequency, style)
- Preferred communication channel and response time expectations
- Who are the decision makers / points of contact
- How we handle changes or scope adjustments

## 5. Information and Access Needed (10 min)
- Documents, credentials, or data needed from the client
- Any third-party introductions required
- Timeline for providing these items

## 6. Next Steps (5 min)
- Confirm immediate action items for both sides
- Set the next check-in or milestone date
- Any questions?

---

## Post-Meeting Action Items

| # | Action | Owner | Due |
|---|--------|-------|-----|
| 1 | {action} | {who} | {when} |
| 2 | {action} | {who} | {when} |
| 3 | {action} | {who} | {when} |
```

### Step 7: Cross-Link Everything

Ensure all created notes reference each other using `[[wiki-links]]`:
- Contact note links to project note and mentions invoice folder
- Project note links to contact note
- Welcome email references are in the onboarding checklist
- All notes use consistent client naming

### Step 8: Report What Was Created

Present a summary of everything created to the user.

## Output Format

```
CLIENT ONBOARDING COMPLETE — {Client Name}
============================================

Notes Created in Cloud Brain:
  1. Contact:   people/{Full Name}
  2. Project:   projects/{Client Name} — {Engagement Type}
  3. Checklist: projects/Onboarding Checklist — {Client Name}
  4. Welcome:   drafts/Welcome Email — {Client Name}
  5. Agenda:    drafts/Kickoff Agenda — {Client Name}

Next Steps:
  1. Review and send the welcome email
  2. Schedule the kickoff call
  3. Collect required info from the client
  4. Work through the onboarding checklist

Client Summary:
  - Name: {name}
  - Company: {company}
  - Engagement: {type}
  - Value: ${amount}
  - Start Date: {date}
```

## Example Usage

**User:** "Onboard John Martinez from Apex Solutions. He wants a 3-month consulting engagement at $2,000/month. His email is john@apexsolutions.com."

**AI:** Creates all 5 notes in Cloud Brain: contact entry with full info, project file scoped to a 3-month consulting engagement at $6,000 total, welcome email referencing consulting deliverables, onboarding checklist customized for consulting (adding items like "confirm project scope", "schedule kickoff call", "share intake questionnaire"), and a kickoff agenda focused on goals and timeline. Reports everything created.

**User:** "New client: Sarah Chen, strategy consulting engagement, $5K/month retainer"

**AI:** Creates full onboarding package for a consulting client. Adapts the checklist and agenda for a retainer-style engagement. Welcome email references monthly deliverables and check-in cadence.

**User:** "I just closed a deal with Marcus Thompson. Set everything up for him."

**AI:** Asks for minimal missing info (company, engagement type, contact details) then runs the full onboarding workflow.

**User:** "Bring on ABC Corp as a client — they need a full marketing package"

**AI:** Creates contact entry for the company, project file for the marketing package work, and all onboarding materials. Adapts the checklist to include project-specific items (brand assets, target audience info, campaign goals).

## Files Created Summary

Every client onboarding creates exactly 5 notes in Cloud Brain:

| # | Note | Folder | Purpose |
|---|------|--------|---------|
| 1 | Contact Entry | `people` | Full contact record with relationship context |
| 2 | Project File | `projects` | Scope, milestones, tasks, timeline |
| 3 | Welcome Email Draft | `drafts` | Ready-to-send welcome email (reads brand voice from CLAUDE.md) |
| 4 | Onboarding Checklist | `projects` | Pre-kickoff, during kickoff, post-kickoff items |
| 5 | Kickoff Meeting Agenda | `drafts` | Structured agenda for the first meeting |

## Onboarding Checklist Detail

The checklist includes these universal items (plus engagement-specific items added dynamically):

### Pre-Kickoff
- [ ] Contact entry created in Cloud Brain (people folder)
- [ ] Project file created in Cloud Brain (projects folder)
- [ ] Contract signed (or agreement in place)
- [ ] Payment received (initial deposit or first invoice)
- [ ] Access granted (any tools, portals, or shared resources)
- [ ] Kickoff call scheduled
- [ ] Welcome email sent
- [ ] Intro email sent to relevant team members

### During Kickoff
- [ ] Goals and expectations aligned
- [ ] Timeline and milestones confirmed
- [ ] Communication preferences established
- [ ] Key contacts identified
- [ ] Required documents/access requested

### Post-Kickoff
- [ ] Meeting notes filed
- [ ] Action items documented in project file
- [ ] First deliverable/milestone scheduled
- [ ] Follow-up email sent with meeting summary
- [ ] Invoice configuration set up (if applicable)

## Error Handling

- **If user does not provide a client name:** Ask specifically: "What is the client's name? I need at least a first and last name to create their records."
- **If user does not provide an engagement type or service description:** Ask: "What type of work are you doing for this client? (e.g., consulting, project work, coaching, design, etc.) This helps me customize the onboarding materials."
- **If a contact entry already exists for this person:** Use `search_notes` to check first. If found, do not create a duplicate. Read the existing note and inform the user: "A contact entry already exists for {name}. I'll update their existing record with the new client engagement info rather than creating a duplicate." Then use `write_note` to update the existing note with the new engagement context.
- **If CLAUDE.md cannot be read for brand voice:** Fall back to a professional, direct tone for the welcome email. Note to user: "I couldn't read your CLAUDE.md for brand voice, so I used a professional default. Feel free to adjust the tone."
- **If the user provides partial info (missing email, phone, etc.):** Proceed with what is available. Fill missing fields with "TBD" in the contact entry. Note in the onboarding checklist: "[ ] Collect missing contact info: {list of missing fields}."
- **If the user wants to onboard a company (not an individual):** Create the contact note using the company name as the title (e.g., "ABC Corp") with a "Primary Contact" field. Adjust the welcome email salutation to address the primary contact person if known, or use the company name.
- **If the user cancels midway or asks to redo:** Any notes already created remain in place. The user can re-run the command to regenerate specific notes, or manually update what they do not want.
