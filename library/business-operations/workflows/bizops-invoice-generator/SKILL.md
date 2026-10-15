---
name: bizops-invoice-generator
description: >-
  Use when the user needs to generate a professional invoice — say 'create an invoice for [client]', 'bill [name] for [amount]', 'invoice [client] for services', or 'make an invoice'. Collects client info, line items, and rates, then outputs a formatted invoice ready to send — saved to Cloud Brain and written as a print-ready HTML file. Also handles updating existing invoices and tracking payment status.
---

# Invoice Generator

## Overview

Generates professional invoices as structured markdown files and HTML files. The user provides client details, services rendered, amounts, and payment terms. The AI produces a clean, formatted invoice with automatic calculations (subtotals, tax, totals), unique invoice numbering, and payment instructions. Invoices are saved to Cloud Brain for tracking and also written as HTML files to a local directory for printing/PDF conversion.

## When This Skill Applies

- User asks to create, generate, or make an invoice
- User wants to bill a client for services
- User mentions invoicing, billing, or creating a bill
- User says "invoice {client name} for {amount/service}"
- User wants a payment receipt or billing document
- User asks about outstanding invoices or payment tracking
- User needs to re-issue or update an existing invoice


## Pre-Flight — Invoice Preferences

1. Search Cloud Brain: `search_notes` with query `"bizops invoice preferences"` (folder: `brain/preferences`)
2. **If found:** Display as a table, confirm, then proceed
3. **If not found:** Ask in ONE message:
   - Business name and billing address
   - Invoice number prefix and starting number (e.g., INV-2026-001)
   - Default payment terms (Net 15 / Net 30 / Due on receipt)
   - Default currency
   - Tax rate (if applicable)
   - Late fee policy (if any)
   - Save to Cloud Brain: `write_note` → title: `bizops-invoice-preferences`, folder: `brain/preferences`
4. Apply throughout: auto-populate all invoice headers, number sequences, and payment terms from preferences
5. Show banner at top of every output:
   ```
   🧾 Invoice Generator | {Business Name} | {Payment Terms} | Prefix: {INV-PREFIX} | Preferences: ✓ loaded
   ```
   Add: *"Say 'update my invoice preferences' to change settings."*

## How It Works

### Step 1: Gather Invoice Details

Extract from the user's message (ask for anything missing that is essential):

**Required:**
| Field | Description |
|-------|-------------|
| Client Name | Person or company being billed |
| Services/Items | What was delivered or provided |
| Amounts | Dollar amount for each line item |

**Optional (use defaults if not provided):**
| Field | Default |
|-------|---------|
| Invoice Date | Today's date |
| Due Date | Net 30 (30 days from invoice date) |
| Invoice Number | Auto-generated: `INV-{YYYYMMDD}-{sequence}` |
| Tax Rate | 0% (no tax unless specified) |
| Payment Terms | Net 30 |
| Payment Method | Check, Zelle, wire, or as configured |
| Notes | None |
| From (Business) | Read from brain config or ask |

### Step 2: Look Up Business Info

Check for existing business configuration in Cloud Brain:

1. Use `search_notes` with query "invoice config" to find the user's business info
2. If no config exists, ask the user for their business info on the first invoice, then save it using `write_note` with title `Invoice Configuration`, folder `invoices`, tags `["config", "invoices"]`

Business config content:
```markdown
# Invoice Configuration

## Business Info
- **Business Name:** {name}
- **DBA:** {dba if any}
- **Address:** {street, city, state, zip}
- **Phone:** {phone}
- **Email:** {email}
- **Website:** {url}

## Payment Methods
- **Zelle:** {email or phone}
- **Wire:** {bank, routing, account}
- **Check payable to:** {name}
- **Other:** {PayPal, Venmo, etc.}

## Default Terms
- **Payment Due:** Net 30
- **Late Fee:** 0%
- **Tax Rate:** 0%

## Invoice Numbering
- **Prefix:** INV
- **Last Number:** {auto-updated}
```

### Step 3: Look Up Client Info

Use `search_notes` with the client name to check if the client exists in Cloud Brain (people folder) for address and contact details. If found, auto-populate the client's billing info.

### Step 4: Calculate Totals

| Calculation | Formula |
|------------|---------|
| Line Item Total | quantity x unit price |
| Subtotal | sum of all line item totals |
| Tax | subtotal x tax rate |
| Discount | subtotal x discount rate (if any) |
| **Total Due** | subtotal + tax - discount |

### Step 5: Generate Invoice Number

Format: `INV-{YYYYMMDD}-{NNN}`

1. Use `search_notes` with query "invoice config" to find the Invoice Configuration note and check the last used number
2. Increment by 1
3. Update the config note with the new last number using `write_note`

If no config exists, start at `INV-{YYYYMMDD}-001`.

### Step 6: Generate the Invoice

Save the invoice to Cloud Brain using `write_note` with:
- **title:** `Invoice {INV-YYYYMMDD-NNN} — {Client Name}`
- **folder:** `invoices`
- **tags:** `["invoice", "client-name-slug", "unpaid"]`

Also write an HTML version to the local filesystem at `~/invoices/{client-slug}/INV-{YYYYMMDD}-{NNN}.html` using the `Write` tool for printing/PDF conversion.

### Step 7: Update Invoice Log

Maintain a master invoice log in Cloud Brain. Use `search_notes` with query "invoice log" to find it, or create it with `write_note`:
- **title:** `Invoice Log`
- **folder:** `invoices`
- **tags:** `["invoice", "log", "tracking"]`

```markdown
# Invoice Log

| Invoice # | Date | Client | Amount | Status | Due Date |
|-----------|------|--------|--------|--------|----------|
| INV-20260302-001 | 2026-03-02 | Meridian Group | $5,000.00 | Sent | 2026-04-01 |
```

Append the new invoice to this log.

### Currency Support

Support multiple currencies. Default to USD unless the user specifies otherwise.

| Currency | Symbol | Code | Format Example |
|----------|--------|------|---------------|
| US Dollar | $ | USD | $1,234.56 |
| Euro | EUR | EUR | EUR 1.234,56 |
| British Pound | GBP | GBP | GBP 1,234.56 |
| Canadian Dollar | CAD | CAD | CAD $1,234.56 |
| Australian Dollar | AUD | AUD | AUD $1,234.56 |
| Mexican Peso | MXN | MXN | MXN $1,234.56 |

If the user says "invoice in euros" or "bill in GBP", use the appropriate currency symbol and code throughout the invoice. Store the currency in the invoice config for consistency.

## Output Format — Markdown Version

The markdown version is saved to Cloud Brain as the invoice note content:

```markdown
# INVOICE

---

**From:**
{Business Name}
{Address Line 1}
{City, State ZIP}
{Phone} | {Email}
{Website}

---

**Bill To:**
{Client Name}
{Client Company (if applicable)}
{Client Address}
{Client Email}

---

| | |
|---|---|
| **Invoice Number** | {INV-YYYYMMDD-NNN} |
| **Invoice Date** | {YYYY-MM-DD} |
| **Due Date** | {YYYY-MM-DD} |
| **Payment Terms** | {Net 30 / Due on receipt / etc.} |
| **Currency** | {USD / EUR / GBP / etc.} |

---

## Services

| # | Description | Qty | Unit Price | Amount |
|---|-------------|-----|-----------|--------|
| 1 | {Service description} | {qty} | ${price} | ${total} |
| 2 | {Service description} | {qty} | ${price} | ${total} |
| 3 | {Service description} | {qty} | ${price} | ${total} |

---

| | |
|---|---|
| **Subtotal** | ${subtotal} |
| **Tax ({rate}%)** | ${tax} |
| **Discount** | -${discount} |
| **TOTAL DUE** | **${total}** |

---

## Payment Instructions

{Payment method details — Zelle, wire, check, etc.}

Please include invoice number **{INV-YYYYMMDD-NNN}** with your payment.

---

## Notes

{Any additional notes, late fee policy, thank you message}

---

*Thank you for your business.*
```

## Output Format — HTML Version (Primary)

Also save an HTML version to `~/invoices/{client-slug}/INV-{YYYYMMDD}-{NNN}.html` that can be opened in any browser and printed or saved as PDF. Use the same HTML template structure with professional styling including:
- Company header with contact info (right-aligned)
- "INVOICE" title (bold, large)
- Bill-To section with client details
- Invoice details table (number, date, due date, terms, status badge)
- Line items table with dark header row, clean borders, right-aligned amounts
- Totals section (right-aligned: subtotal, tax, discount, bold total)
- Payment instructions in a light gray box
- Notes section
- "Thank you for your business" footer
- Print-optimized @media print styles

**To convert to PDF:** Open the HTML file in any web browser, press Ctrl/Cmd+P, and select "Save as PDF". The styles are print-optimized.

## Example Usage

**User:** "Invoice Jane Doe at Meridian Group for a consulting fee — $4,500"

**AI:** Checks for existing business config and client info in Cloud Brain. Generates invoice INV-20260302-001 with one line item: "Consulting Services — Project Strategy" for $4,500. Saves to Cloud Brain in the `invoices` folder and writes HTML to `~/invoices/meridian-group/`. Updates the invoice log.

**User:** "Create an invoice for Sarah Johnson. Services: Project Management $2,000 and document preparation $500. Net 15 terms."

**AI:** Generates invoice with 2 line items totaling $2,500, Net 15 due date, saves to Cloud Brain and writes HTML.

**User:** "Bill XYZ Corp for consulting — 10 hours at $150/hour"

**AI:** Generates invoice with line item "Consulting Services — 10 hours @ $150/hr" totaling $1,500.

**User:** "Show me all outstanding invoices"

**AI:** Uses `search_notes` with query "invoice" and tag "unpaid" to find all outstanding invoices. Presents the list.

**User:** "Mark invoice INV-20260302-001 as paid"

**AI:** Finds the invoice note in Cloud Brain using `search_notes`, updates its content with a "PAID" stamp and payment date, and changes the tag from "unpaid" to "paid". Updates the invoice log note similarly.

**User:** "Invoice Nexus Solutions for 3 consulting engagements at EUR 2,000 each, plus document prep EUR 500. Due in 15 days. Use EUR."

**AI:** Generates invoice with 2 line items: "Consulting Services (3 engagements @ EUR 2,000)" = EUR 6,000 and "Document Preparation" = EUR 500. Total: EUR 6,500. Due date: 15 days from today. Both markdown (Cloud Brain) and HTML versions saved with EUR formatting.

## Error Handling

- **If invoice config does not exist in Cloud Brain (first invoice):** Ask the user for their business info (name, address, phone, email, payment methods). Create the config note using `write_note`. Inform them: "I saved your business info so future invoices will auto-populate. You can update it anytime."
- **If user does not provide a client name:** Ask specifically: "Who should this invoice be billed to? I need at least a client name."
- **If user does not provide amounts or services:** Ask specifically: "What services or items should I include on this invoice, and what are the amounts?"
- **If user provides ambiguous amounts (e.g., "a few thousand"):** Ask for exact numbers: "I need specific dollar amounts for each line item. Can you give me the exact figures?"
- **If the invoice number sequence has a gap or conflict:** Search Cloud Brain for all existing invoices to determine the true next number. Always trust the actual notes over the config.
- **If an invoice note already exists with the same number:** Do not overwrite. Increment the sequence number and inform the user: "INV-{number} already exists. Created as INV-{next number} instead."
- **If the client has no entry in Cloud Brain:** Proceed without it, but suggest: "I don't have {client name} in your contacts. Want me to create a contact entry for them?"
- **If tax rate is unclear:** Default to 0% (no tax) and note on the invoice: "Tax not included. Consult your accountant for applicable tax obligations."
- **If currency is not recognized:** Default to USD and inform: "I didn't recognize the currency '{input}'. Using USD. Supported currencies: USD, EUR, GBP, CAD, AUD, MXN."
- **If the user asks to edit an existing invoice:** Find the invoice in Cloud Brain using `search_notes`, read it, make the requested changes, update the "Last Modified" date, and add a note in the invoice log: "Revised on {date}". Do not delete the original — create a revision suffix if needed (e.g., INV-20260302-001-R1).
- **If writing the HTML file fails:** Fall back to Cloud Brain-only output and inform the user: "HTML generation encountered an issue. Saved the markdown version to Cloud Brain. You can still view it there."
