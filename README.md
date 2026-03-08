# Airlock Playbooks

Operational workflows, agent task templates, and runbooks for the Airlock platform.

## What's Here

| Playbook | Path | Purpose |
|----------|------|---------|
| **Otto Prospect Research** | `prospecting/otto-research.md` | Research a prospect with Otto, enrich data, generate outreach |
| **New Workspace Setup** | `onboarding/new-workspace.md` | Set up a new Airlock workspace from scratch |
| **Vault Lifecycle** | `contracts/vault-lifecycle.md` | Move work through Discover > Build > Review > Ship |
| **Agent Task Templates** | `agent-tasks/templates.md` | Standard formats for the agent coordination queue |

## For Agents

This repo is an MCP read-only server. Agents in any Airlock repo can query playbooks but cannot modify them from outside. To update a playbook, work inside this repo directly.

Check `pack.json` for the structured index of all playbooks.

## Playbook Format

Every playbook follows this structure:

- **Trigger**: When to use this playbook
- **Modules used**: Which Airlock modules are involved
- **Roles required**: Who can execute this
- **Steps**: Specific, actionable instructions
- **Otto Integration**: How the AI assistant helps at each step
- **Outputs**: What this playbook produces
