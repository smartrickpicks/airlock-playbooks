# Playbook: Otto Prospect Research

**Trigger**: You have a target company or contact you want to evaluate and engage.
**Modules used**: CRM, Documents, Tasks
**Roles required**: Any module role with Otto access (Builder, Gatekeeper, Owner)
**Estimated time**: 5-15 minutes per prospect (Otto does the heavy lifting)
**Otto tools used**: search_corpus, deal_fields, vault_timeline, draft_patch, web_lookup, contact_enrichment

---

## Overview

This is the core prospecting workflow. You give Otto a company name, URL, or contact, and Otto researches them, enriches the data, identifies key contacts, generates talking points, and drafts outreach -- all within a CRM Vault. By the end, you have a fully enriched prospect record with a ready-to-send outreach sequence.

---

## Steps

### Step 1: Create or Open a CRM Vault

Open the CRM module. Either create a new Vault for this prospect or open an existing one.

**New Vault**: Use the quick-create flow. Minimum required fields:
- Vault name (company name)
- Chamber: starts in **Discover**
- At least one tag: `prospect`, `partner`, `investor` (depending on the target type)

**Existing Vault**: If you've already logged this company, open it and confirm you're in the Discover chamber.

### Step 2: Trigger Otto Research

In the Signal panel, open Otto and use one of these prompts:

**For a company you know by name:**
> Research [Company Name]. Find their website, what they do, company size, recent news, funding history, and key decision makers. Focus on [specific angle -- e.g., "their AI/automation stack" or "partnership opportunities in contract management"].

**For a company you found via URL:**
> Research this company: [URL]. Pull everything you can -- what they do, who runs it, recent activity, and why they might need what we're building.

**For a specific person:**
> Research [Person Name] at [Company Name]. Find their role, background, LinkedIn presence, recent public activity, and what they care about professionally. I want to reach out about [topic].

### Step 3: Otto Executes the Research Pipeline

Otto runs the enrichment pipeline automatically. Here's what happens under the hood (you don't need to do anything -- just watch the Signal panel):

1. **Web lookup** -- Otto searches for the company/person across public sources
2. **Contact enrichment** -- Pulls available professional data (role, company, location, social profiles)
3. **Search corpus** -- Checks if we have any existing knowledge about this entity in the OGC
4. **Deal fields** -- Populates the Vault's structured fields (territory, entity type, counterparty info)

Otto will present findings in a structured format in the Signal panel. Review what comes back.

**What to look for:**
- Is the company real and active?
- Are the contacts actually decision-makers or gatekeepers?
- Is there a clear angle for why they'd want Airlock (or whatever you're selling)?
- Any red flags (dormant company, bad press, misaligned industry)?

### Step 4: Refine and Verify

Otto's research is a draft. You are the human in the loop. Review and correct:

**If something is wrong or missing**, tell Otto:
> That's not the right [Person]. The CEO is actually [Name]. Update the contact.

**If you need deeper research on a specific angle:**
> Dig deeper into their tech stack. What tools are they using for [specific area]? Check their job postings for clues.

**If the prospect looks good, ask for talking points:**
> Based on this research, give me 3-5 talking points for why Airlock would matter to them. Focus on [their pain point].

### Step 5: Generate Outreach Draft

Once you're satisfied with the research and talking points, ask Otto to draft outreach:

**For email:**
> Draft a cold outreach email to [Contact Name]. Keep it under 150 words. Lead with [specific value prop or mutual connection]. Tone: [professional but human / casual / formal]. Include a clear CTA for a 15-minute call.

**For LinkedIn:**
> Draft a LinkedIn connection request message to [Contact Name]. Max 300 characters. Reference [shared interest or mutual connection].

**For a follow-up sequence:**
> Create a 3-touch outreach sequence for [Contact Name]: initial email, LinkedIn follow-up 3 days later, second email 5 days after that. Each touch should reference a different talking point.

Otto will create these as draft patches (per the Security Constitution -- Otto drafts, you approve). Review each draft in the Orchestrate panel.

### Step 6: Approve and Stage

Review Otto's drafts. Edit anything that doesn't sound like you. Then:

1. **Approve the patches** -- This moves the drafts from "AI-drafted" to "human-approved" status
2. **Update Vault fields** -- Confirm the enriched data is correct in the deal fields
3. **Create follow-up tasks** -- In the Tasks module, create tasks for each outreach touch with due dates
4. **Move to Build chamber** -- If this prospect is worth pursuing, advance the Vault from Discover to Build (this triggers the Gate check -- see the contracts/vault-lifecycle playbook)

### Step 7: Log and Track

Everything is already logged in the Vault's timeline (append-only audit trail). But explicitly:

- Tag the Vault with the outreach status: `outreach-staged`, `outreach-sent`, `replied`, `meeting-booked`
- Set a follow-up date in the Calendar module
- If you're tracking pipeline value, update the deal fields with estimated value

---

## Otto Integration Details

### Tools Used at Each Step

| Step | Otto Tools | Notes |
|------|-----------|-------|
| Research | web_lookup, contact_enrichment, search_corpus | All read-only. No external writes. |
| Field population | deal_fields, draft_patch | draft_patch creates proposals, not committed changes |
| Talking points | search_corpus, deal_fields | Otto synthesizes from enriched data |
| Outreach drafts | draft_patch | All drafts require human approval |
| Timeline | vault_timeline | Read-only review of what's happened |

### PII Handling

Per the Security Constitution (Principle 9): PII is redacted before reaching external LLM providers. When Otto sends context to Claude/GPT for research synthesis:

- Contact names are replaced with indexed placeholders: `[CONTACT_1]`, `[CONTACT_2]`
- Email addresses are redacted entirely
- Company financial data is summarized, not passed raw
- The redaction map is stored locally so responses can be de-redacted for display

When using Ollama (local model), redaction is bypassed since data never leaves the machine.

### Safety Constraints

- Otto cannot send emails or messages autonomously (Principle 3: no autonomous AI actions)
- Otto cannot modify CRM records without patch approval (draft-only)
- Max 5 tool calls per Otto message (circuit breaker)
- If enrichment sources fail, Otto degrades gracefully and tells you what it couldn't find

---

## Outputs

By the end of this playbook, you have:

- A fully enriched CRM Vault with company data, contacts, and deal fields
- Verified talking points tailored to the prospect
- Draft outreach (email, LinkedIn, or sequence) ready to send after your approval
- Follow-up tasks with due dates in the Tasks module
- A complete audit trail of the research process in the Vault timeline
