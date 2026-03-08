# Playbook: Vault Lifecycle (Chamber Transitions)

**Trigger**: A Vault exists and needs to progress through its lifecycle, OR you need to understand how gate transitions work.
**Modules used**: Any module that uses Vaults (Contracts, CRM, Tasks)
**Roles required**: Builder (to do work), Gatekeeper (to approve transitions), Owner (for high-risk transitions)
**Estimated time**: Varies by vault complexity. A simple prospect vault might take hours. A contract vault might take weeks.

---

## Overview

Every Vault moves through four Chambers in sequence: **Discover > Build > Review > Ship**. Between each Chamber is a Gate -- a checkpoint that requires specific criteria to be met and specific approvals before the Vault can advance. This playbook defines what happens in each Chamber, what the Gate criteria are, and who can approve.

The Chamber model is inspired by manufacturing airlocks: you can't open the next door until the current one is sealed. This prevents half-finished work from leaking forward and ensures quality at every stage.

---

## The Four Chambers

```
 DISCOVER          BUILD            REVIEW           SHIP
+----------+    +----------+    +----------+    +----------+
|          |    |          |    |          |    |          |
| Research |    | Create   |    | Verify   |    | Execute  |
| Explore  |--->| Draft    |--->| Approve  |--->| Close    |
| Assess   |    | Refine   |    | Sign-off |    | Archive  |
|          |    |          |    |          |    |          |
+----------+    +----------+    +----------+    +----------+
      |    GATE 1    |    GATE 2    |    GATE 3    |
      |   (assess)   |   (review)   |   (approve)  |
```

---

## Chamber 1: Discover

**Purpose**: Gather information, assess the opportunity, determine if this Vault is worth pursuing.

**What happens here**:
- Research the subject (company, contact, deal, project)
- Enrich data using Otto and the enrichment pipeline
- Populate initial Vault fields (deal fields, tags, members)
- Identify stakeholders and assign preliminary roles
- Assess risk level: Low, Medium, High, or Critical

**Otto's role in Discover**:
- Run the prospecting research pipeline (see `prospecting/otto-research.md`)
- Enrich contacts and company data
- Search the OGC corpus for related knowledge
- Suggest risk classification based on deal size, complexity, and counterparty

**Who works here**: Builders (research and data entry), Owners (oversight)

**Key outputs**:
- Enriched Vault with populated fields
- Risk classification assigned
- Preliminary member/role assignments
- Assessment notes in the Vault timeline

### Gate 1: Discover -> Build

**Gate name**: Readiness Assessment

**Criteria** (all must be met):

| Criterion | Description | Verified By |
|-----------|-------------|-------------|
| Fields populated | Required deal fields are not empty (company, contact, territory at minimum) | System check |
| Risk classified | Vault has a risk level assigned (Low/Medium/High/Critical) | Builder |
| Assessment complete | At least one assessment note exists in the timeline | System check |
| Member assigned | At least one Builder is assigned for the Build phase | Owner or Gatekeeper |

**Approval required**:
- **Low risk**: 1 Gatekeeper
- **Medium risk**: 1 Gatekeeper
- **High risk**: 1 Gatekeeper + 1 Owner
- **Critical risk**: All Gatekeepers + 1 Owner

**Self-approval prevention**: The person who performed the Discover research cannot approve their own Gate transition. A different Gatekeeper must approve. (For solo users with approval workflows disabled, this gate is logged but not enforced.)

---

## Chamber 2: Build

**Purpose**: Create the deliverables. Draft contracts, build proposals, prepare outreach sequences, develop the work product.

**What happens here**:
- Draft documents (contracts, proposals, SOWs, outreach)
- Build task lists and timelines
- Configure workflow specifics (approval chains, deadlines)
- Iterate on drafts with Otto assistance
- Collect internal feedback before formal review

**Otto's role in Build**:
- Draft documents using templates from the skills library and document packs
- Generate clause suggestions from the OGC corpus
- Create task breakdowns and timelines
- Provide drafting assistance (rewording, formatting, compliance checks)
- All Otto outputs are draft patches -- nothing is committed without human approval

**Who works here**: Builders (primary creators), Designers (layout and presentation), Owners (direction)

**Key outputs**:
- Complete draft documents ready for review
- Task list with assignments and deadlines
- All draft patches approved by the Builder (internal approval, not Gate approval)
- Version history in the Vault timeline

### Gate 2: Build -> Review

**Gate name**: Completeness Review

**Criteria** (all must be met):

| Criterion | Description | Verified By |
|-----------|-------------|-------------|
| Deliverables exist | At least one document or work product is attached to the Vault | System check |
| No open draft patches | All AI-drafted patches have been either approved or rejected by a Builder | System check |
| Tasks assigned | If tasks exist, they have assignees and due dates | System check |
| Builder sign-off | The primary Builder has marked the work as "ready for review" | Builder (explicit action) |

**Approval required**:
- **Low risk**: 1 Gatekeeper
- **Medium risk**: 1 Gatekeeper + 1 Owner
- **High risk**: 2 Gatekeepers + 1 Owner
- **Critical risk**: All Gatekeepers + all Owners + 24-hour cooling period

---

## Chamber 3: Review

**Purpose**: Independent verification. The work product is examined by people who didn't create it. This is where quality, compliance, and accuracy are validated.

**What happens here**:
- Gatekeepers review all deliverables
- Compliance checks against templates and standards
- Counterparty review (if external parties need to approve)
- Red-line edits and tracked changes
- Risk re-assessment (did anything change since Discover?)

**Otto's role in Review**:
- Compare drafts against templates for compliance (read-only analysis)
- Highlight deviations from standard clauses or terms
- Summarize changes since the last review
- Otto does NOT approve or make review decisions -- that's human-only

**Who works here**: Gatekeepers (primary reviewers), Owners (oversight), Viewers (read-only stakeholders)

**Key outputs**:
- Review comments and tracked changes
- Compliance verification (pass/fail per criterion)
- Risk re-assessment (confirmed or updated)
- Gatekeeper sign-off

### Gate 3: Review -> Ship

**Gate name**: Final Approval

**Criteria** (all must be met):

| Criterion | Description | Verified By |
|-----------|-------------|-------------|
| All reviews complete | Every assigned Gatekeeper has submitted their review | System check |
| No blocking comments | All comments marked as "blocking" have been resolved | System check |
| Risk confirmed | Risk level has been re-confirmed or updated after review | Gatekeeper |
| Compliance passed | All compliance checks pass (if applicable) | Gatekeeper |
| Owner approval | At least one Owner has approved the final version | Owner (explicit action) |

**Approval required**:
- **Low risk**: 1 Gatekeeper + 1 Owner
- **Medium risk**: 1 Gatekeeper + 1 Owner
- **High risk**: 2 Gatekeepers + 1 Owner + 24-hour cooling period
- **Critical risk**: All Gatekeepers + all Owners + 48-hour cooling period + Executive sign-off

**Cooling period**: For High and Critical risk, the Gate cannot be approved until the cooling period has elapsed after the last review comment is resolved. This prevents rush-to-approve pressure.

---

## Chamber 4: Ship

**Purpose**: Execute, deliver, close. The work product goes live -- contracts get signed, outreach gets sent, projects launch.

**What happens here**:
- Execute the deliverables (send contracts, launch campaigns, deploy)
- Capture execution evidence (signed documents, sent confirmations, delivery receipts)
- Close out tasks
- Archive the Vault with full timeline

**Otto's role in Ship**:
- Generate execution summaries
- Create follow-up tasks for post-execution monitoring
- Archive and index the Vault's OGC chunks for future reference
- Otto does NOT execute autonomous actions (Principle 3) -- it assists with preparation

**Who works here**: Builders (execution), Owners (final sign-off), Admin (archival)

**Key outputs**:
- Executed deliverables with evidence
- Closed tasks
- Archived Vault with complete timeline
- OGC chunks indexed for future knowledge retrieval

---

## Reverse Transitions

Vaults can move backward if issues are found:

| Transition | When | Who Can Trigger |
|-----------|------|-----------------|
| Review -> Build | Blocking issues found during review | Gatekeeper |
| Build -> Discover | Fundamental assumptions changed | Owner |
| Ship -> Review | Post-execution issue discovered | Owner or Executive |

Reverse transitions are logged in the audit trail with mandatory justification. They cannot be done silently.

---

## Vault Lifecycle Audit Trail

Every action in a Vault's lifecycle is logged to the append-only audit trail (Security Principle 5):

- Chamber transitions (forward and reverse) with approver identity
- Gate criteria checks (pass/fail per criterion)
- Document versions and changes
- Otto interactions (tool calls, drafts, approvals)
- Role changes and member assignments
- Risk classification changes

The audit trail is immutable. It cannot be edited or deleted. It provides the complete decision lineage for any work product that moved through the Vault.
