# Agent Task Templates

**Purpose**: Standard formats for tasks that agents pick up from the coordination queue. Every task in `airlock-coordination/queue.json` follows one of these templates. This ensures consistent handoffs, clear acceptance criteria, and auditable completion.

---

## Task Format

Every task in the queue uses this structure:

```json
{
  "id": "task-[timestamp]-[short-hash]",
  "template": "[template-name]",
  "title": "[human-readable title]",
  "assigned_to": "[agent-id or 'unassigned']",
  "assigned_by": "orchestrator",
  "priority": "P0 | P1 | P2 | P3",
  "status": "queued | in-progress | review | complete | blocked",
  "repo": "[which repo this task targets]",
  "files_affected": ["list of files this task will touch"],
  "context": "[link to relevant doc, spec, or prior task]",
  "acceptance_criteria": ["list of specific, verifiable criteria"],
  "created": "[ISO timestamp]",
  "started": "[ISO timestamp or null]",
  "completed": "[ISO timestamp or null]",
  "blocker": "[description of blocker or null]",
  "output_summary": "[filled by agent on completion]"
}
```

### Priority Levels

| Priority | Meaning | Response Time |
|----------|---------|---------------|
| **P0** | Blocks other agents or the entire sprint | Pick up immediately |
| **P1** | Important for current phase completion | Pick up within current session |
| **P2** | Needed but not blocking | Pick up when P0/P1 are clear |
| **P3** | Nice to have, can defer | Pick up if capacity allows |

### Status Flow

```
queued -> in-progress -> review -> complete
                  |                    ^
                  v                    |
               blocked ---------> (unblocked) -> in-progress
```

---

## Template: Feature Implementation

**Template name**: `feature-impl`

Use when: An agent needs to build a new feature or complete an existing one.

```json
{
  "template": "feature-impl",
  "title": "Implement [feature name]",
  "repo": "airlock-app",
  "context": "Reference spec: [link to feature spec in airlock-docs]",
  "acceptance_criteria": [
    "Feature works as described in the spec",
    "All new API endpoints have auth middleware and workspace_id scoping",
    "Frontend components follow existing Tailwind/Zustand patterns",
    "No TypeScript errors (pnpm type-check passes)",
    "No lint errors (pnpm lint passes)",
    "Manual smoke test passes: [describe the test]"
  ]
}
```

**Agent workflow**:
1. Read the referenced spec
2. Check if related files are locked in `coordination/locks.json`
3. Lock the files you'll modify
4. Implement the feature
5. Run `pnpm type-check` and `pnpm lint`
6. Write an output summary describing what was built and any decisions made
7. Set status to `review` and notify the Orchestrator

---

## Template: Bug Fix

**Template name**: `bug-fix`

Use when: Something is broken and needs to be fixed.

```json
{
  "template": "bug-fix",
  "title": "Fix: [description of the bug]",
  "repo": "airlock-app",
  "context": "Bug report: [description of what's wrong, steps to reproduce]",
  "acceptance_criteria": [
    "The described bug no longer occurs",
    "No regressions in related functionality",
    "Root cause documented in output summary",
    "If the bug was in a security-sensitive area, note that in the summary"
  ]
}
```

**Agent workflow**:
1. Reproduce the bug (or understand it from the description)
2. Identify root cause
3. Fix it with minimal changes (don't refactor while fixing)
4. Verify the fix
5. Document root cause and fix in output summary
6. Set status to `review`

---

## Template: Security Sub-Spec Implementation

**Template name**: `security-impl`

Use when: A security sub-spec has been written in airlock-docs and needs to be implemented in code.

```json
{
  "template": "security-impl",
  "title": "Implement [security spec name]",
  "repo": "airlock-app",
  "context": "Security spec: [link to spec in airlock-docs/Features/Security/]",
  "acceptance_criteria": [
    "Implementation matches the spec exactly -- no deviations without Orchestrator approval",
    "All Security Constitution principles are respected",
    "Encryption uses the specified algorithm and key derivation",
    "No plaintext PII in logs, error messages, or API responses",
    "Security-sensitive code has inline comments explaining why",
    "Output summary includes a security checklist sign-off"
  ]
}
```

**Agent workflow**:
1. Read the security spec thoroughly
2. Read the Security Constitution (10 principles) -- verify nothing conflicts
3. Implement exactly as specified
4. If you find a conflict or ambiguity: STOP and ask the Orchestrator. Do not guess on security.
5. Document every security decision in the output summary
6. Set status to `review` -- security tasks always get Orchestrator review

---

## Template: Documentation

**Template name**: `docs-write`

Use when: Documentation needs to be written or updated.

```json
{
  "template": "docs-write",
  "title": "Write/update [document name]",
  "repo": "airlock-docs",
  "context": "Related to: [what prompted this doc -- a feature, a decision, a question that kept coming up]",
  "acceptance_criteria": [
    "Uses canonical Airlock vocabulary (check vocabulary table in Master Strategy)",
    "Cross-references related specs where appropriate",
    "Follows existing doc format and structure in the repo",
    "No placeholder content or TODOs left in the document",
    "Accurate as of the current state of the codebase"
  ]
}
```

---

## Template: Repo Setup

**Template name**: `repo-setup`

Use when: A new repo needs to be created or an existing one restructured. CLI Agent only.

```json
{
  "template": "repo-setup",
  "title": "Create/setup [repo name]",
  "repo": "[target repo]",
  "context": "Architecture spec: [link to Master Strategy Part 2]",
  "acceptance_criteria": [
    "AGENTS.md exists and accurately describes what the repo owns",
    "CLAUDE.md exists and loads AGENTS.md",
    "README.md exists with human-facing overview",
    "Directory structure matches the architecture spec",
    ".gitignore is appropriate for the repo's tech stack",
    "Initial files are committed (not empty placeholder)",
    "No sensitive data in the initial commit"
  ]
}
```

---

## Template: MCP Wiring

**Template name**: `mcp-wire`

Use when: An MCP connection between repos needs to be established or verified.

```json
{
  "template": "mcp-wire",
  "title": "Wire MCP: [source repo] -> [target repo]",
  "repo": "[source repo]",
  "context": "MCP Registry: airlock-config/registry.json",
  "acceptance_criteria": [
    "Source can read from target via MCP",
    "Write access matches the registry spec (read-only or read-write as defined)",
    "Connection survives a container restart",
    "Error handling works when the target is unavailable (graceful degradation)",
    "No credentials are transmitted via MCP payloads"
  ]
}
```

---

## Template: Playbook Creation

**Template name**: `playbook-write`

Use when: A new operational playbook needs to be written.

```json
{
  "template": "playbook-write",
  "title": "Write playbook: [playbook name]",
  "repo": "airlock-playbooks",
  "context": "Needed for: [what workflow or use case this serves]",
  "acceptance_criteria": [
    "Follows the standard playbook format (trigger, modules, roles, time, steps, otto integration, outputs)",
    "Steps are specific and actionable (not vague guidance)",
    "Otto integration section specifies which tools are used at each step",
    "PII handling is addressed if the playbook involves external data",
    "A new user could follow this playbook without prior Airlock experience"
  ]
}
```

---

## Agent Checklist (run this for every task)

Before starting:
- [ ] Read the task and all referenced context
- [ ] Check `coordination/locks.json` for conflicts
- [ ] Lock files you'll modify
- [ ] Register your session in `coordination/state.json`

While working:
- [ ] Stay within the scope of the task (don't fix unrelated things)
- [ ] If blocked, set status to `blocked` with description -- do NOT work around it
- [ ] If a security question arises, ask the Orchestrator -- do NOT guess

On completion:
- [ ] Write the output summary (what you did, decisions made, anything the reviewer should know)
- [ ] Release file locks
- [ ] Set status to `review`
- [ ] Notify the Orchestrator
