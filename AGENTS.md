# airlock-playbooks — Agent Entry Point

## What This Repo Owns
Operational workflows, deployment procedures, agent task templates, runbooks, OTTO workflow definitions, and quality gates.

## MCP Mode
Read-only server. All repos can read playbooks. Only agents in this repo write.

## Directory Structure
- `deployment/` — Deployment procedures and infrastructure runbooks
- `prospecting/` — Prospect research, outreach sequences, follow-up workflows
- `onboarding/` — New user and workspace setup guides
- `contracts/` — Contract lifecycle workflows (Discover > Build > Review > Ship)
- `agent-tasks/` — Agent task templates for automated workflows

## Coordination
- Register your session in airlock-coordination before starting work
- Playbooks are versioned — never modify in place, create new versions
- Changes require Orchestrator review before merge

## Vocabulary
See airlock-docs for the canonical glossary. Key terms: Module, Vault, Chamber, Gate, Pack, Controller, Maverick, Builder, Otto.

## Ingestion
Local reference files are available at `~/Desktop/Airlock/ingestion/`:
- `documents/` — Source documents for extraction, analysis, or spec drafting
- `research/` — External research, competitor analysis, market data
- `imports/` — Files being imported into the platform (contracts, templates, datasets)

All agents across the constellation can read from this shared ingestion folder via the `airlock-ingestion` MCP server.
