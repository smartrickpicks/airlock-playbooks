# Playbook: New Workspace Setup

**Trigger**: A new user (Controller) is starting their Airlock for the first time, OR you're creating an additional workspace.
**Modules used**: Admin, all modules during configuration
**Roles required**: Org-level Executive or Director (workspace creator gets Executive by default)
**Estimated time**: 20-30 minutes for basic setup, 1-2 hours for full configuration

---

## Overview

This playbook takes a new Controller from "I just signed up" to "I have a working Airlock with modules configured, roles assigned, and my first Vault created." It follows the Chamber metaphor: the setup itself moves through Discover (explore what's available), Build (configure your workspace), Review (verify it works), and Ship (start using it).

---

## Prerequisites

Before starting, the user needs:

- An Airlock account (Google OAuth sign-in)
- A clear idea of what they're using Airlock for (prospecting, contract management, project tracking, etc.)
- If adding team members: their email addresses

---

## Steps

### Phase 1: Discover (Explore What's Available)

#### Step 1.1: Sign In and Create Workspace

Sign in via Google OAuth. On first login, you'll be prompted to create a workspace.

- **Workspace name**: Your company or team name. This becomes the top-level tenant boundary. All data is isolated to this workspace.
- **You are automatically assigned the Executive org role** -- full access to everything.

#### Step 1.2: Connect MCP Nodes

This is where you choose which Airlock modules power your workspace. The setup flow reads from `airlock-config/registry.json` and presents the available nodes:

| Node | What It Adds | Recommended For |
|------|-------------|-----------------|
| airlock-docs | Documentation, vocabulary, architecture reference | Always on (connected by default) |
| airlock-skills-library | UI components, agent skills, templates, toolkits | Anyone customizing their interface or workflows |
| airlock-playbooks | Operational workflows, task templates, runbooks | Anyone using guided workflows (most users) |
| airlock-coordination | Multi-agent state, file locks, task queues | Anyone running multiple AI agents |
| airlock-gen-ui | AI-generated interfaces, dynamic UI | Advanced users building custom views |
| airlock-config | Registry, setup flow, defaults | Always on (connected by default) |

For most new Controllers: connect **docs**, **playbooks**, and **skills-library**. Add coordination and gen-ui later if needed.

#### Step 1.3: Choose Your Modules

Enable the application modules you need. All modules are available but you only activate what's relevant:

| Module | Use Case | Enable If... |
|--------|----------|-------------|
| **CRM** | Contact and company management, pipeline tracking | You're doing sales, prospecting, or relationship management |
| **Contracts** | Contract lifecycle, clause management, approvals | You handle agreements, SOWs, or legal documents |
| **Tasks** | Kanban boards, task tracking, assignments | You need project/task management (most users) |
| **Calendar** | Schedule view computed from vault dates | You want timeline visibility across vaults |
| **Documents** | Rich text editing, PDF viewing, document management | You work with documents regularly |
| **Admin** | System overlay for workspace management | Always available (not a toggle) |

For prospecting: enable **CRM**, **Tasks**, and **Documents** at minimum.

### Phase 2: Build (Configure Your Workspace)

#### Step 2.1: Set Up Your Role Structure

If you're a solo user, you can skip this -- you have Executive access to everything. If you're adding team members, define your role structure now.

**Org-level roles** (set the ceiling for what a user can do anywhere):
- **Executive**: Full access. Workspace owner.
- **Director**: Near-full access. Cannot modify workspace settings.
- **Lead**: Can manage within assigned modules. Cannot change org settings.
- **Member**: Standard access. Works within assigned module roles.

**Module-level roles** (per-module, additive):
- **Owner**: Full control of the module
- **Gatekeeper**: Can approve gate transitions and patches
- **Builder**: Can create and modify content
- **Designer**: Can modify appearance and layout
- **Viewer**: Read-only access

Assign yourself as Owner on all active modules. You can refine later.

#### Step 2.2: Invite Team Members (Optional)

If adding people now:

1. Go to Admin > Members
2. Invite by email (they'll sign in via Google OAuth)
3. Assign org role (start with Member -- you can promote later)
4. Assign module roles per module

Remember: **self-approval prevention is hardcoded**. If you're the only Gatekeeper, you cannot approve your own gate transitions. Either add a second Gatekeeper or operate without gate enforcement during solo use.

#### Step 2.3: Configure Feature Flags

The Feature Control Plane has 27 flags. For a new workspace, the defaults are sensible. But review these key flags:

- **Otto enabled**: Turn on to use the AI assistant
- **Real-time collaboration**: Turn on if multiple users will work simultaneously
- **Approval workflows**: Turn on if you want Gate enforcement (requires 2+ users for self-approval prevention)
- **Document generation**: Turn on if using the Documents module

Access via Admin > Feature Control Plane. Each flag shows its current state, description, and rollout percentage.

#### Step 2.4: Set Up Your Custom Content Layer

This is where the content resolution hierarchy comes in. Your workspace has two content sources:

1. **Platform defaults** (read-only) -- standard skills, playbooks, and templates that ship with Airlock
2. **Your custom layer** (read-write) -- your modifications, additions, and overrides

To customize: navigate to the workspace settings and find the content configuration. Anything you add to your custom layer takes priority over platform defaults.

For now, the platform defaults are sufficient. Customize as you learn what you need.

### Phase 3: Review (Verify It Works)

#### Step 3.1: Create Your First Vault

Go to your primary module (e.g., CRM) and create a Vault:

- **Name**: Something real -- a prospect you actually want to research, or a contract you're actually working on
- **Chamber**: Starts in Discover
- **Assign yourself** as a member with Builder + Gatekeeper roles

This is your test. Don't use dummy data -- use a real case so you validate the workflow with something you care about.

#### Step 3.2: Test Otto

Open the Signal panel in your Vault and talk to Otto:

> Summarize what we know about this vault so far.

Otto should respond with the vault's current state. If Otto is working, try the prospecting playbook (see `prospecting/otto-research.md`).

If Otto doesn't respond: check that the Otto feature flag is enabled and that your model routing is configured (Admin > AI Runtime).

#### Step 3.3: Test the Triptych Layout

Verify all three panels work:

- **Signal** (left): Shows incoming data, Otto chat, notifications
- **Orchestrate** (center): Shows the vault's workflow state, editable fields, documents
- **Control** (right): Shows settings, audit trail, member management, AI context

Navigate between them. Make sure data flows correctly.

#### Step 3.4: Verify Permissions

If you added team members, test that:
- Members can only access modules they have roles in
- Viewers can see but not edit
- Gate approvals require the right role
- Self-approval prevention works (try approving your own patch -- it should be blocked if you created it)

### Phase 4: Ship (Start Using It)

#### Step 4.1: Run Your First Real Workflow

Pick a playbook and run it for real:

- **Prospecting**: Follow `prospecting/otto-research.md` with a real target
- **Contracts**: Follow `contracts/vault-lifecycle.md` with a real agreement
- **General**: Create tasks, set due dates, assign to team members

#### Step 4.2: Customize as You Go

As you work, you'll discover what needs customizing:

- **New playbook needed?** Draft it and add to your custom content layer
- **Module role doesn't fit?** Adjust the role assignments
- **Otto needs different behavior?** Modify the system prompts in Admin > AI Runtime
- **Missing a document template?** Add it to your custom documents

#### Step 4.3: Set Up Your Rhythm

Establish daily habits:

- **Morning**: Check Signal panel for new notifications, review task due dates
- **Work blocks**: Use Otto for research and drafting, work in Orchestrate panel
- **End of day**: Review what changed (Control panel audit trail), update task statuses

---

## Outputs

By the end of this playbook, you have:

- A configured workspace with your chosen modules active
- MCP nodes connected for docs, playbooks, and skills
- Role structure defined (even if solo -- you're Owner/Gatekeeper on everything)
- At least one real Vault created and tested
- Otto verified and working
- A clear understanding of how Signal / Orchestrate / Control map to your workflow

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Otto not responding | Check Admin > Feature Control Plane: is Otto enabled? Check Admin > AI Runtime: is model routing configured? |
| Can't create a Vault | Verify you have Builder role on the target module |
| Gate transition blocked | Self-approval prevention: you need a different user to approve, or disable approval workflows for solo use |
| MCP node not connecting | Check Admin > MCP Connections: is the node URL correct and reachable? |
| Missing playbooks | Verify airlock-playbooks is connected as an MCP source in your workspace settings |
