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

## MVP Sample Usage

Below are concrete examples of how to use the subagents for a typical MVP build.

### 1. Scope a new MVP from a rough idea

Use `product_owner` first when the product is still vague.

Sample prompt:

```text
Use the product_owner subagent.
I want to build an MVP for a tutor marketplace for parents booking 30-minute reading sessions for kids.
Please define the smallest shippable scope, core user stories, acceptance criteria, and what to defer until later.
```

### 2. Turn the MVP scope into a technical shape

Use `solution_architect` once the scope is clear and you need data models, boundaries, or contracts.

Sample prompt:

```text
Use the solution_architect subagent.
Based on this MVP: parents can sign up, browse tutors, book a session, and pay.
Propose the minimum backend modules, data model, API surface, and repo structure for a solo developer.
Keep it intentionally small.
```

### 3. Ask for an execution order before coding

Use `solo_tech_lead` when the work crosses product, backend, web, and mobile concerns.

Sample prompt:

```text
Use the solo_tech_lead subagent.
I have an MVP with web signup, tutor search, booking, and a simple parent dashboard.
Create the recommended execution order, delegation map, major risks, and definition of done.
Assume I am building this alone and want the fewest moving parts possible.
```

### 4. Build a single web MVP slice

Use `web_developer` directly when the requirements are already decided.

Sample prompt:

```text
Use the web_developer subagent.
Implement the MVP parent signup flow with:
- email and password fields
- validation errors
- loading and success states
- redirect to the dashboard after success
Keep changes localized and consistent with the existing stack.
```

### 5. Build a mobile MVP slice

Use `mobile_developer` when the feature is mobile-specific or app-lifecycle-aware.

Sample prompt:

```text
Use the mobile_developer subagent.
Implement the MVP booking confirmation screen in the mobile app.
It should show tutor name, time, price, booking status, and a button to return to the dashboard.
Handle loading, empty, and error states cleanly.
```

### 6. Validate the MVP before merge

Use `qa_test_engineer` after implementation to get focused coverage and regression thinking.

Sample prompt:

```text
Use the qa_test_engineer subagent.
Review this MVP booking flow and produce:
- a risk-based test plan
- critical happy path checks
- edge cases
- regression risks
- any missing automated tests worth adding now
```

### 7. Run a release-readiness pass

Use `security_release_manager` before shipping anything that touches auth, payments, secrets, or storage.

Sample prompt:

```text
Use the security_release_manager subagent.
Review the MVP for release readiness.
Focus on auth, secret handling, payment-related risk, storage of personal data, and obvious security gaps.
Return ship / no-ship guidance with required fixes first.
```

### 8. Full MVP multi-agent chain

Use a short chain when you want the whole MVP flow from idea to release review.

Sample sequence:

1. `product_owner`: define the smallest shippable MVP.
2. `solution_architect`: define minimum architecture and contracts.
3. `solo_tech_lead`: sequence the work and assign ownership.
4. `web_developer`: implement the web slice.
5. `mobile_developer`: implement the mobile slice if needed.
6. `qa_test_engineer`: validate risk and coverage.
7. `security_release_manager`: run final release review.

Sample parent prompt:

```text
Use the product_owner, solution_architect, and solo_tech_lead subagents first.
The product is an MVP for parents to book reading tutors for children.
I want the smallest viable launch plan, minimum architecture, and a practical execution sequence.
After that, hand off the first implementation task to web_developer.
```

### 9. Single-agent MVP shortcut

If the task is already narrow, skip orchestration.

Examples:

- `web_developer`: "Build the landing page and waitlist form for the MVP."
- `mobile_developer`: "Implement the first-run onboarding carousel for the MVP app."
- `qa_test_engineer`: "Review the signup and booking flows for missing edge-case coverage."

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
- example task transcripts, although the sample prompts above can serve as starter usage patterns

If you want to operationalize this repository further, the next useful additions would be:

- a loader or manifest format for your agent runtime
- sample prompts showing when to use each agent
- versioning or changelog notes for prompt revisions
- evaluation cases for consistency and handoff quality
