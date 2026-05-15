# agentX44

`agentX44` is a repository of Codex subagent definitions for solo developers building, validating, and securing web and mobile products.

The repository is organized into two agent packs:

- `mvp-agents/` - product, architecture, implementation, QA, and release agents for building MVPs.
- `security/` - mobile security audit agents for Android, iOS, and mixed mobile repositories.

Each agent is stored as a standalone `.toml` file with metadata such as `name`, `description`, `model`, `sandbox_mode`, nickname candidates, and embedded `developer_instructions`.

## Repository Structure

| Path | Purpose |
| --- | --- |
| `mvp-agents/` | Seven agents for moving from product idea to implementation, validation, and release review. |
| `security/` | One security orchestrator plus ten focused mobile security audit layer agents. |
| `docs/` | Handoff documents generated or consumed by planning agents, such as MVP plans and architecture plans. |
| `AGENT.md` | Operator reference for the agent set. |

This repository contains agent definitions only. It does not include a custom loader, CLI, package registry, or automated prompt test runner.

## MVP Agent Pack

Use `mvp-agents/` when you are defining or building a product feature.

Recommended full workflow:

1. `product_owner` - define the smallest shippable MVP scope, user stories, acceptance criteria, assumptions, and open questions.
2. `solution_architect` - turn the approved MVP plan into architecture, boundaries, data models, contracts, and implementation sequencing.
3. `solo_tech_lead` - organize execution order, delegation, risks, and definition of done.
4. `web_developer` and/or `mobile_developer` - implement the decided web or mobile slices.
5. `qa_test_engineer` - validate behavior, edge cases, regression risk, and test coverage.
6. `security_release_manager` - review release readiness, secrets, auth, storage, network, dependencies, rollback, and monitoring.

Example prompt:

```text
Use the product_owner subagent.
I want to build an MVP for parents to book reading tutors for children.
Define the smallest launch scope, user stories, acceptance criteria, and what to defer.
```

For narrow tasks, use the relevant specialist directly:

```text
Use the web_developer subagent.
Implement the MVP signup form with validation, loading, error, and success states.
```

See [`mvp-agents/README.md`](mvp-agents/README.md) for the full agent table and usage examples.

## Security Agent Pack

Use `security/` when you need a source-and-configuration security audit for Android, iOS, or mixed mobile projects.

Recommended full workflow:

1. `security_orchestrator` - detect the mobile platform, coordinate the L1-L10 audit layers, aggregate findings, and write `SECURITY_AUDIT_REPORT.md`.
2. Focused layer agents - run a targeted audit when you only need one security area reviewed, such as secrets, auth, dependencies, network, IPC, or logging.

Example full audit prompt:

```text
Use the security_orchestrator subagent.
Audit this Android/iOS project for mobile security vulnerabilities and write SECURITY_AUDIT_REPORT.md.
```

Example focused audit prompt:

```text
Use the security_l4_secrets subagent.
Audit only secrets and build-security risk in this mobile project.
```

See [`security/README.md`](security/README.md) for the audit layers, expected report format, and operating rules.

## How To Use The Agents

Reference the desired agent by its `name` from the TOML file in your prompt:

```text
Use the <agent_name> subagent.
<Describe the task, constraints, expected output, and any relevant files or docs.>
```

Good prompts usually include:

- the product, feature, or repository being worked on
- the current state of the work
- the exact output you want
- constraints such as platform, stack, deadline, or release risk
- whether the agent should only review or may edit files

Agent sandbox defaults matter:

- Planning and implementation agents that need to write handoff docs or code use `workspace-write`.
- Review and audit agents are usually `read-only`.
- The security orchestrator can write the final `SECURITY_AUDIT_REPORT.md`, but the layer agents are designed to audit without fixing files.

## Common Workflows

### New MVP From A Rough Idea

```text
Use the product_owner, solution_architect, and solo_tech_lead subagents first.
The product is an MVP for parents to book reading tutors for children.
I want the smallest viable launch plan, minimum architecture, and a practical execution sequence.
After that, hand off the first implementation task to web_developer.
```

### Implementation-Only Web Task

```text
Use the web_developer subagent.
Build the dashboard empty state and loading state for the existing web app.
Keep the changes localized and consistent with the current UI system.
```

### Implementation-Only Mobile Task

```text
Use the mobile_developer subagent.
Implement the booking confirmation screen in the mobile app.
Show tutor name, time, price, booking status, and a button back to the dashboard.
```

### Pre-Release Review

```text
Use the qa_test_engineer and security_release_manager subagents.
Review the completed signup and booking flow before release.
Return release blockers first, then the smallest test and security checklist needed before shipping.
```

### Mobile Security Audit

```text
Use the security_orchestrator subagent.
Run a full source-and-configuration mobile security audit.
Write SECURITY_AUDIT_REPORT.md with evidence-backed findings, severity, exact files, line numbers, fixes, and references.
```

## Design Principles

The agent set is designed around:

- narrow responsibilities
- small, reviewable outputs
- explicit assumptions
- solo-builder practicality
- evidence-backed review
- handoffs that another agent or human can use without needing the full chat history
