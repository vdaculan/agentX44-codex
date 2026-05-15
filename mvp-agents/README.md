# MVP Agents

`mvp-agents/` contains seven Codex subagent definitions for building web and mobile products with a solo-developer workflow.

The pack covers the path from product idea to release:

1. product scope
2. architecture
3. execution planning
4. implementation
5. QA
6. security and release review

Each agent is a standalone `.toml` file. Use the agent by referencing its `name` in your prompt.

## Agents

| Agent | File | Purpose | Sandbox |
| --- | --- | --- | --- |
| `product_owner` | `product_owner.toml` | Turns rough ideas into MVP scope, user stories, acceptance criteria, assumptions, and a handoff-ready MVP plan. | `workspace-write` |
| `solution_architect` | `solution_architect.toml` | Reviews the MVP plan, identifies technical gaps, and defines architecture, contracts, data models, and build sequencing. | `workspace-write` |
| `solo_tech_lead` | `solo_tech_lead.toml` | Creates execution order, delegation maps, risk calls, and definitions of done for solo-builder delivery. | `read-only` |
| `web_developer` | `web_developer.toml` | Implements web pages, components, forms, dashboards, auth flows, API integration, and responsive frontend behavior. | `workspace-write` |
| `mobile_developer` | `mobile_developer.toml` | Implements Flutter, Android Kotlin, and iOS Swift screens, navigation, state, device features, and API integration. | `workspace-write` |
| `qa_test_engineer` | `qa_test_engineer.toml` | Produces test plans, acceptance validation, regression checks, edge cases, and automation recommendations. | `read-only` |
| `security_release_manager` | `security_release_manager.toml` | Reviews security, secrets, auth, storage, network, dependencies, release readiness, rollback, and monitoring. | `read-only` |

## Recommended Workflow

Use the full chain when the work starts from an idea or spans multiple surfaces:

1. Start with `product_owner` to define the smallest valuable MVP.
2. Use `solution_architect` after the MVP scope is approved.
3. Use `solo_tech_lead` when the work needs sequencing across product, backend, web, mobile, QA, or release.
4. Hand implementation to `web_developer` and/or `mobile_developer`.
5. Use `qa_test_engineer` to validate behavior and regression risk.
6. Use `security_release_manager` before release, especially for auth, payments, personal data, storage, network, or dependency changes.

For small tasks, skip unnecessary roles and use the specialist directly.

## Usage Pattern

Prompt format:

```text
Use the <agent_name> subagent.
<Describe the goal, constraints, relevant files, expected output, and whether edits are allowed.>
```

Example:

```text
Use the product_owner subagent.
I want to build an MVP for a tutor marketplace for parents booking 30-minute reading sessions for kids.
Define the smallest shippable scope, core user stories, acceptance criteria, and what to defer.
```

## Agent Examples

### Product Owner

Use `product_owner` when the product or feature is still vague.

```text
Use the product_owner subagent.
I want to build an MVP for a habit tracking app for busy professionals.
Define the target user, MVP scope, user stories, acceptance criteria, success metrics, assumptions, and open questions.
```

Expected output:

- product goal
- target user
- MVP scope
- out-of-scope items
- user stories
- acceptance criteria
- edge cases and business rules
- success metrics
- an MVP plan section that can be saved under `docs/`

### Solution Architect

Use `solution_architect` when the product scope is known and the implementation needs a technical shape.

```text
Use the solution_architect subagent.
Based on the approved MVP plan in docs/, define the minimum architecture, data model, API boundaries, state ownership, security considerations, and build sequence.
```

Expected output:

- MVP-to-architecture gap check
- unresolved implementation blockers
- architecture summary
- recommended stack and rationale
- data model and API contract notes
- build sequence recommendation
- an architecture doc section that can be saved under `docs/`

### Solo Tech Lead

Use `solo_tech_lead` when the work needs coordination or execution order.

```text
Use the solo_tech_lead subagent.
I have an MVP plan and architecture plan for signup, search, booking, and a simple dashboard.
Create the execution order, delegation map, key risks, and definition of done for a solo developer.
```

Expected output:

- objective
- scope and out-of-scope
- recommended execution order
- agent delegation map
- key decisions needed
- risks and mitigations
- definition of done

### Web Developer

Use `web_developer` when the requirements are ready and the work is browser-facing.

```text
Use the web_developer subagent.
Implement the MVP parent signup flow with email and password fields, validation errors, loading and success states, and redirect to the dashboard after success.
Keep changes localized and consistent with the existing stack.
```

Expected output:

- brief implementation plan
- files changed or proposed
- key behavior implemented
- validation performed
- remaining risks or follow-up items

### Mobile Developer

Use `mobile_developer` when the work touches Flutter, Android, iOS, app lifecycle, navigation, local storage, permissions, or device behavior.

```text
Use the mobile_developer subagent.
Implement the MVP booking confirmation screen in the mobile app.
Show tutor name, time, price, booking status, and a button to return to the dashboard.
Handle loading, empty, and error states cleanly.
```

Expected output:

- brief implementation plan
- files changed or proposed
- key behavior implemented
- validation performed
- platform-specific notes
- remaining risks or follow-up items

### QA Test Engineer

Use `qa_test_engineer` after implementation or when requirements need validation coverage.

```text
Use the qa_test_engineer subagent.
Review this MVP booking flow and produce a risk-based test plan, happy path checks, edge cases, regression risks, and automated tests worth adding now.
```

Expected output:

- test objective
- scope under test
- risk-based test matrix
- happy path cases
- edge, error, permission, and concurrency cases
- regression checklist
- suggested automation targets
- release recommendation

### Security Release Manager

Use `security_release_manager` before release or when the work touches sensitive behavior.

```text
Use the security_release_manager subagent.
Review the MVP for release readiness.
Focus on auth, secret handling, payment-related risk, storage of personal data, dependency risk, rollback, and post-release monitoring.
Return ship or no-ship guidance with required fixes first.
```

Expected output:

- security review scope
- findings grouped by severity
- release blockers
- safe-to-ship conditions
- release checklist
- rollback or hotfix guidance
- monitoring and post-release watch items

## Shortcut Workflows

Use one agent directly when the task is already narrow:

- `web_developer`: build or fix a web screen, component, form, dashboard, or integration.
- `mobile_developer`: build or fix a mobile screen, navigation flow, permission flow, or device-aware feature.
- `qa_test_engineer`: produce a test plan or regression review for a completed feature.
- `security_release_manager`: run a lightweight release readiness pass.

Use the planning chain when the task is broad:

```text
Use the product_owner, solution_architect, and solo_tech_lead subagents.
Start from this product idea, create the smallest MVP plan, convert it into a minimum architecture, and produce the first implementation handoff.
```

## Write Behavior

- `product_owner` can write approved MVP handoff docs under `docs/`.
- `solution_architect` can write approved architecture handoff docs under `docs/`.
- `web_developer` and `mobile_developer` can edit implementation files in the workspace.
- `solo_tech_lead`, `qa_test_engineer`, and `security_release_manager` are read-only by default.

Always state clearly in the prompt whether the agent should only review or may make changes.
