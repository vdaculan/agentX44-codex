# AGENT.md

This file is the operator reference for the `agentX44` agent set.

## Purpose

`agentX44` defines seven specialized Codex subagents for a solo developer workflow that spans product definition, architecture, implementation, QA, and release review.

## Agent Reference

### `product_owner`
- Primary use: MVP scoping, PRDs, user stories, acceptance criteria, prioritization
- Model: `gpt-5.4`
- Sandbox: `workspace-write`
- Best used when: the idea is still fuzzy and needs clear scope before code starts
- Special behavior: ends with an MVP Markdown plan, asks for missing context, and can save the approved plan into `docs/` for `solution_architect`

### `solution_architect`
- Primary use: system design, MVP-to-architecture gap checks, API boundaries, repo structure, data modeling, technical tradeoffs
- Model: `gpt-5.4`
- Sandbox: `workspace-write`
- Best used when: multiple components need a shared contract or architecture decision
- Special behavior: reviews the MVP doc in `docs/`, surfaces technical blockers and unresolved decisions, and can save the approved architecture handoff doc into `docs/`

### `solo_tech_lead`
- Primary use: sequencing, delegation, risk management, cross-functional planning
- Model: `gpt-5.4`
- Sandbox: `read-only`
- Best used when: the task is broad, cross-platform, or needs execution order

### `web_developer`
- Primary use: web implementation for pages, components, forms, dashboards, auth flows, API integration
- Model: `gpt-5.4`
- Sandbox: `workspace-write`
- Best used when: the work is frontend or browser-facing and ready to build

### `mobile_developer`
- Primary use: Flutter, Android, iOS implementation, device-aware behavior, navigation, permissions, async flows
- Model: `gpt-5.4`
- Sandbox: `workspace-write`
- Best used when: the task touches mobile UI, app lifecycle, local storage, or device features

### `qa_test_engineer`
- Primary use: test planning, risk-based validation, regression coverage, edge-case discovery
- Model: `gpt-5.4`
- Sandbox: `read-only`
- Best used when: you need independent verification before merging or releasing

### `security_release_manager`
- Primary use: security review, secret handling, auth/storage/network checks, release gating
- Model: `gpt-5.4`
- Sandbox: `read-only`
- Best used when: the feature is approaching release or changes security-sensitive behavior

## Suggested Usage Order

Use the full chain only when the task needs it:

1. `product_owner`
2. `solution_architect`
3. `solo_tech_lead`
4. `web_developer` and/or `mobile_developer`
5. `qa_test_engineer`
6. `security_release_manager`

For small tasks, skip unnecessary roles. The prompts are already written to favor minimal scope and solo-builder efficiency.

## Repository Facts

- All current agent definitions are plain `.toml` files in the repository root.
- Each file includes structured metadata plus a long `developer_instructions` block.
- The planning and review agents are mostly `read-only`, except `product_owner` and `solution_architect`, which can persist approved handoff docs in `docs/`.
- The implementation agents are `workspace-write`.

## Maintenance Guidance

When updating an agent file:

- keep responsibilities narrow and non-overlapping
- keep output shapes explicit so handoffs stay consistent
- preserve guardrails that prevent scope creep and unsafe edits
- change models or sandbox levels intentionally, since those are operational choices

## Missing Pieces

This repository does not yet define:

- a loader or registry for these agent configs
- automated tests for prompt quality
- examples of real task routing
- semantic versioning for prompt changes

If this repo becomes a reusable internal package, those would be the next additions.
