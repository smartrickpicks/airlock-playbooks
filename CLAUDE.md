# airlock-playbooks

Operational workflows and runbooks for the Airlock platform.

## Read First
Read AGENTS.md for your role and boundaries.

## MCP Connections
- Reads from: airlock-docs (vocabulary, specs)
- Provides to: all repos (read-only)

## Rules
- Playbooks describe HOW to do things (docs describe WHAT, skills describe WITH-WHAT)
- Use canonical vocabulary from airlock-docs
- Include pre-conditions, steps, post-conditions, and rollback for each playbook
- Tag playbooks with the persona they serve: Controller, Maverick, or Builder
