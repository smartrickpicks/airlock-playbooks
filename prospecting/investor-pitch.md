# Playbook: Investor Pitch Through Airlock

**Trigger**: You have an investor meeting coming up, OR you're building a pipeline of investor targets.
**Modules used**: CRM (investor tracking), Documents (pitch materials), Triage (prep tasks), Calendar (meeting timeline)
**Roles required**: Any role with Builder + Otto access
**Estimated time**: 2-4 hours for first investor, 30-60 minutes for each additional (templates compound)

---

## Overview

This playbook does two things at once: it prepares you for investor conversations AND demonstrates Airlock's power by using Airlock to do the preparation. You're dogfooding the product in the highest-stakes scenario possible — fundraising. Every step happens inside a Vault, which means every piece of research, every draft, every decision is tracked in your audit trail.

By the end, you have: a researched investor profile, tailored talking points, pitch materials built in Documents, a prep task checklist in Triage, and a complete record of the process that you can replicate for every investor on your list.

---

## The Vault Lifecycle for Investor Pitches

Each investor gets their own Vault in the CRM module. The Vault moves through Chambers:

- **Discover**: Research the investor, understand their thesis, find warm paths
- **Build**: Create tailored pitch materials, rehearse talking points
- **Review**: Self-review your materials (or have a team member review)
- **Ship**: Deliver the pitch, track follow-ups, close the round

---

## Steps

### Phase 1: Discover (Research the Investor)

#### Step 1.1: Create an Investor Vault

In the CRM module, create a new Vault:

- **Name**: Investor name or firm name (e.g., "Sequoia — [Partner Name]")
- **Tags**: `investor`, `seed` or `series-a` (whatever stage you're targeting)
- **Chamber**: Discover (default)

#### Step 1.2: Otto Deep Research

Open Signal panel and ask Otto to research the investor:

> Research [Investor/Firm Name]. I need:
> 1. Their investment thesis — what do they look for?
> 2. Recent investments in the last 12 months — especially anything in AI, enterprise software, or workflow automation
> 3. The specific partner I should target and why
> 4. Any public statements they've made about the future of AI in business operations
> 5. Portfolio companies that could be customers, partners, or comparisons for Airlock

Otto runs the enrichment pipeline and returns structured findings. Review them in Signal.

#### Step 1.3: Find the Angle

Ask Otto to connect the dots:

> Based on this investor's thesis and portfolio, what's the strongest angle for pitching Airlock? Consider:
> - Which Airlock differentiator matters most to them? (vault lifecycle, MCP architecture, marketplace model, security-first, AI-as-recall-not-creation)
> - Is there a portfolio company that validates the problem Airlock solves?
> - Is there a portfolio company that could be a customer?
> - What objections would this investor likely raise?

Review Otto's analysis. Edit and refine — this is your strategic lens for the pitch.

#### Step 1.4: Map the Warm Path

Ask Otto:

> Do we have any connections to [Firm] or [Partner Name]? Check our CRM for mutual contacts, shared communities, or past interactions. Also check if any of their portfolio founders are in our network.

If no warm path exists, note that and plan a cold approach. Tag the Vault: `warm-intro` or `cold-outreach`.

#### Gate 1 Criteria: Ready to Build?

Before moving to Build, verify:
- [ ] Investor thesis understood and documented in Vault fields
- [ ] Target partner identified
- [ ] Angle defined (which Airlock differentiators to lead with)
- [ ] Warm path mapped (or cold approach planned)
- [ ] At least one potential objection identified with a counter

---

### Phase 2: Build (Create Pitch Materials)

#### Step 2.1: Tailor the Pitch Deck

In the Documents module (attached to this Vault), create or fork your master pitch deck. Ask Otto to help tailor it:

> I'm pitching [Investor]. They care about [thesis]. Their biggest portfolio overlap is [company]. Adjust our pitch narrative to:
> - Lead with [the angle from Discover]
> - Include a comparison to [portfolio company] to make it tangible
> - Address [anticipated objection] proactively in the slide flow
> - Keep it to 12 slides max

Otto will draft slide-by-slide talking points. Use these to update your deck. The deck itself lives in Documents with full version history.

#### Step 2.2: Build the One-Pager

If the investor needs a leave-behind or a pre-meeting summary:

> Draft a one-page investor summary for [Investor]. Include:
> - Problem statement (in their language, based on their thesis)
> - Airlock's solution (focused on [angle])
> - Traction (OrcestrateOS POC with Create Music Group, current state of the product)
> - Ask (what you're raising, what it's for)
> - Team (your background: military intelligence, Web3, community building)

Review, edit, approve the draft. Save in Documents.

#### Step 2.3: Prepare the Demo Script

If you're doing a live demo (you should — Airlock is the demo):

> Create a 5-minute demo script that shows Airlock's power. The flow should be:
> 1. Start at Dispatch — show the global view
> 2. Open a CRM Vault — show the Triptych layout (Signal/Orchestrate/Control)
> 3. Ask Otto to research a company — show AI-powered enrichment in real-time
> 4. Show the Chamber lifecycle — Discover through Ship
> 5. End on the marketplace vision — Packs, Mavericks, the platform play
>
> Keep each step to 1 minute. Note what to click and what to say.

#### Step 2.4: Build a Prep Checklist in Triage

Create tasks in the Triage module:

- [ ] Deck tailored for this investor
- [ ] One-pager ready (if needed)
- [ ] Demo environment clean (fresh seed data, no test junk)
- [ ] Demo script rehearsed (run through at least twice)
- [ ] Backup plan if demo breaks (screenshots, recorded walkthrough)
- [ ] Calendar hold sent/confirmed
- [ ] Follow-up email drafted (to send within 2 hours of meeting)

Assign due dates. Track completion.

#### Gate 2 Criteria: Ready for Review?

- [ ] Tailored deck complete
- [ ] One-pager complete (if applicable)
- [ ] Demo script written and rehearsed
- [ ] All prep tasks in Triage marked complete
- [ ] Follow-up email drafted

---

### Phase 3: Review (Verify Everything)

#### Step 3.1: Self-Review (Solo Founder)

If you're solo, do your own review pass:

- Read the deck out loud. Does it flow? Does the story land in under 10 minutes?
- Run through the demo script. Time it. Is it under 5 minutes?
- Read the one-pager. Does it answer the three questions every investor has: Why this? Why now? Why you?
- Check the follow-up email. Is it specific to this investor, not generic?

#### Step 3.2: External Review (If Available)

If you have a trusted advisor, mentor, or team member:
- Share the deck and one-pager from the Vault (they can get Viewer access)
- Ask for honest feedback on: clarity, flow, what's missing, what's too much
- Track their feedback as comments in the Vault timeline

#### Step 3.3: Rehearse the Hard Questions

Ask Otto to play devil's advocate:

> You are a skeptical VC partner at [Firm]. You've seen hundreds of "AI workflow" pitches. Grill me on Airlock. Give me the 5 hardest questions you'd ask, especially around: market size, defensibility, why not just use [Salesforce/HubSpot/Monday], burn rate, and the marketplace timing.

Prepare your answers. Save them in Documents as "Objection Handling — [Investor]."

#### Gate 3 Criteria: Ready to Ship?

- [ ] All materials reviewed (self or external)
- [ ] Feedback incorporated
- [ ] Hard questions prepared with answers
- [ ] Demo runs cleanly end-to-end
- [ ] Calendar confirmed with investor

---

### Phase 4: Ship (Deliver and Follow Up)

#### Step 4.1: The Meeting

Deliver the pitch. Trust the prep. Key principles:

- **Lead with the problem, not the product.** The investor needs to feel the pain before they see the solution.
- **Demo > slides.** Get to the live product as fast as possible. Airlock IS the pitch.
- **Tell the OrcestrateOS story.** Create Music Group said the vision was "too big." You took that as a challenge. That's founder energy investors buy into.
- **End with the marketplace.** This is what turns Airlock from a product into a platform. Controllers, Mavericks, Packs — this is the Shopify analogy that makes the business model click.

#### Step 4.2: Immediate Follow-Up

Within 2 hours of the meeting:

1. Send the pre-drafted follow-up email (edit to reference specific moments from the conversation)
2. Update the Vault: add meeting notes to the timeline, update deal fields (interest level, next steps, concerns raised)
3. Create follow-up tasks in Triage with dates

#### Step 4.3: Track the Pipeline

As you pitch more investors, the CRM module becomes your fundraising pipeline:

- Each investor is a Vault
- Tag Vaults by stage: `initial-outreach`, `meeting-scheduled`, `pitched`, `follow-up`, `term-sheet`, `closed`
- Use Dispatch to see all investor Vaults at a glance
- Otto can summarize your pipeline: "Give me a status update on all investor vaults. Who's warmest? Who needs follow-up? Where am I stuck?"

#### Step 4.4: Close the Vault

When an investor either commits or passes:

- Move the Vault to Ship (if committed) or tag as `passed` and archive
- Log the outcome in the timeline
- If they passed: ask Otto to analyze why and note lessons for the next pitch

---

## Otto Integration Summary

| Phase | Otto Prompt Pattern | Tools Used |
|-------|-------------------|------------|
| Discover | "Research [investor]..." | web_lookup, contact_enrichment, search_corpus |
| Discover | "What's the strongest angle..." | deal_fields, search_corpus |
| Build | "Tailor the pitch narrative..." | draft_patch, search_corpus |
| Build | "Draft a one-pager..." | draft_patch |
| Build | "Create a demo script..." | draft_patch |
| Review | "Play devil's advocate..." | search_corpus, deal_fields |
| Ship | "Summarize my pipeline..." | search_corpus, deal_fields, vault_timeline |

---

## Outputs

By the end of this playbook, you have (per investor):

- A fully researched investor profile with thesis, portfolio analysis, and warm paths
- A tailored pitch deck with investor-specific narrative
- A one-pager leave-behind
- A rehearsed demo script
- Objection handling notes
- A follow-up email ready to send
- Complete audit trail of the entire prep process
- Pipeline tracking across all investor Vaults

And you've demonstrated Airlock's value by using it to prepare the pitch about Airlock. That's the ultimate dogfood.
