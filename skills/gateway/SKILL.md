---
name: gateway
description: Plan and deliver company software changes with task breakdown, verification, PR review, fix review, and local-first data safeguards.
---

# Gateway

Use Gateway for engineering work that needs a deliberate lifecycle from discovery through review. For small, isolated fixes, scale the artifacts down while preserving verification and independent review.

## Operating roles

Use these labels in task plans, handoffs, and findings:

- `gateway:orchestrator` — tracks scope, decisions, and completion evidence.
- `gateway:discovery` — clarifies the goal, constraints, and acceptance criteria.
- `gateway:planner` — creates the approved design and task breakdown.
- `gateway:implementer` — makes scoped changes and records evidence.
- `gateway:test` — defines and executes appropriate checks.
- `gateway:pr-reviewer` — independently reviews the proposed result.
- `gateway:fix-reviewer` — independently verifies review fixes and regressions.
- `gateway:security` — assesses secrets, data handling, permissions, dependencies, and external effects.
- `gateway:sandbox` — verifies risky code and data workflows in an isolated, non-production environment.

Read [the operating policy](references/operating-policy.md) before an action involving external access, sensitive data, credentials, deployment, persistent records, destructive operations, dependencies, or integrations.

## Lifecycle

1. **Discover — `gateway:discovery`:** inspect relevant local context; state the desired outcome, non-goals, constraints, acceptance criteria, and assumptions. Ask a focused question only when it materially changes the work.
2. **Plan — `gateway:planner`:** for work beyond an isolated fix, propose a design and ordered tasks using [the plan template](../../templates/plan.md). Each task must define scope, files/components, verification, owner tag, completion condition, and applicable review flags. Obtain approval before non-trivial implementation.
3. **Sandbox — `gateway:sandbox`:** when a task has `[SANDBOX_REQUIRED]`, validate it with isolated fixtures, test credentials, and no production access. Never use sandboxing as permission to delete persistent records.
4. **Implement — `gateway:implementer`:** take one approved task at a time. Make the smallest adequate change. Add or update tests when behavior changes, and record commands, results, changed files, and limitations. Never execute a record-deletion operation; use the user-run protocol in the operating policy.
5. **Review — `gateway:pr-reviewer`:** independently compare the change with the approved plan, acceptance criteria, tests, safety policy, review flags, and likely edge cases. Record findings with [the review template](../../templates/review.md). Classify them as `blocker`, `major`, `minor`, or `question`.
6. **Fix and re-review — `gateway:implementer`, `gateway:fix-reviewer`:** fix each blocker or major finding, rerun focused and required project checks, then independently verify the original concern and surrounding behavior. Repeat until blockers and major findings are resolved. Defer a security finding only with explicit user approval and a recorded rationale.
7. **Complete — `gateway:orchestrator`:** report completed tasks, verification evidence, findings/resolutions, known limitations, and follow-ups. Do not claim success without evidence.

## Review flags

Set flags during planning, carry them into review, and clear them only with recorded evidence.

- `[SANDBOX_REQUIRED]` — validate in an isolated environment before normal implementation or user handoff.
- `[DATA_SAFETY_REVIEW]` — the task reads, writes, transforms, or exposes persistent records; `gateway:security` must review it.
- `[USER_RUN_REQUIRED]` — the task would delete records or invoke a destructive operation. Gateway must not run it; provide an exact, reviewed command for the user to inspect and run manually.
- `[PRODUCTION_BLOCKED]` — production credentials, production data, and production writes are out of scope for Gateway execution.
- `[EXTERNAL_ACTION_REVIEW]` — the task would call a third-party service, send data, or create an external resource.

## Discrepancies

If requirements, plan, code, tests, review evidence, or observed behavior conflict: stop the affected task, document the conflict using [the discrepancy template](../../templates/discrepancy.md), propose the smallest safe resolution, and ask for direction when the resolution changes scope, external behavior, security posture, cost, or reversibility.

## Status format

```text
Gateway status
- Stage: discovery | planning | implementation | review | fix review | complete
- Active role: gateway:<role>
- Task: <id and outcome>
- Evidence: <checks performed or not run>
- Findings: <open count by severity>
- Discrepancies / decisions needed: <none or details>
```
