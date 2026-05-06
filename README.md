# agentX44

`agentX44` is a lightweight repository of seven specialized Codex subagent definitions for solo developers building web and mobile products.

Each agent is stored as a standalone `.toml` file with:
- a stable agent `name`
- a short `description`
- a preferred `model`
- a `sandbox_mode`
- candidate nicknames
- embedded `developer_instructions` that define role, scope, guardrails, and output shape

## Repository Contents

| File | Purpose |
| --- | --- |
| `product_owner.toml` | Converts ideas into MVP scope, user stories, and acceptance criteria. |
| `solution_architect.toml` | Defines system boundaries, contracts, data models, and technical tradeoffs. |
| `solo_tech_lead.toml` | Orchestrates sequencing, delegation, and cross-functional execution for solo builders. |
| `web_developer.toml` | Implements web pages, components, forms, dashboards, and frontend integration. |
| `mobile_developer.toml` | Implements Flutter, Android, and iOS features with platform-aware behavior. |
| `qa_test_engineer.toml` | Produces test plans, regression analysis, edge cases, and release recommendations. |
| `security_release_manager.toml` | Reviews security, secrets, auth/storage/network risk, and release readiness. |

## Agent Design

The set is intentionally split into three layers:

1. Planning
   - `product_owner`
   - `solution_architect`
   - `solo_tech_lead`
2. Implementation
   - `web_developer`
   - `mobile_developer`
3. Validation and release
   - `qa_test_engineer`
   - `security_release_manager`

This separation keeps responsibilities explicit and reduces overlap between requirement writing, architecture, implementation, testing, and ship/no-ship review.

## Model and Sandbox Profile

- Planning and review agents mostly use `gpt-5.4` with `high` reasoning.
- Implementation agents use `gpt-5.3-codex-spark` with `medium` reasoning for faster execution.
- Writer/reviewer roles are configured as `read-only`.
- Builder roles are configured as `workspace-write`.

This makes the repository suitable for an agentic workflow where only implementation agents are expected to edit files directly.

## Recommended Workflow

For a new feature or product slice:

1. Start with `product_owner` to define the smallest buildable scope.
2. Use `solution_architect` if contracts, data shape, or repo boundaries need clarification.
3. Use `solo_tech_lead` when the work spans multiple surfaces or needs sequencing.
4. Hand off implementation to `web_developer` and/or `mobile_developer`.
5. Run `qa_test_engineer` for validation coverage and regression risk.
6. Finish with `security_release_manager` before release.

For small implementation-only tasks, `web_developer` or `mobile_developer` can be used directly without full orchestration.

## Shared Conventions Across Agents

Across the seven files, the prompts consistently emphasize:

- clear scope boundaries
- minimal, reviewable changes
- explicit assumptions
- maintainability over novelty
- solo-builder practicality
- output structures that are easy to hand off between agents

## Current State

This repository contains agent definitions only. It does not currently include:

- runtime code to load these `.toml` files
- scripts for agent registration or packaging
- tests for prompt behavior
- example task transcripts

If you want to operationalize this repository further, the next useful additions would be:

- a loader or manifest format for your agent runtime
- sample prompts showing when to use each agent
- versioning or changelog notes for prompt revisions
- evaluation cases for consistency and handoff quality
