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

Read [the operating policy](references/operating-policy.md) before an action involving external access, sensitive data, credentials, deployment, destructive operations, dependencies, or integrations.

## Lifecycle

1. **Discover — `gateway:discovery`:** inspect relevant local context; state the desired outcome, non-goals, constraints, acceptance criteria, and assumptions. Ask a focused question only when it materially changes the work.
2. **Plan — `gateway:planner`:** for work beyond an isolated fix, propose a design and ordered tasks using [the plan template](../../templates/plan.md). Each task must define scope, files/components, verification, owner tag, and completion condition. Obtain approval before non-trivial implementation.
3. **Implement — `gateway:implementer`:** take one approved task at a time. Make the smallest adequate change. Add or update tests when behavior changes, and record commands, results, changed files, and limitations.
4. **Review — `gateway:pr-reviewer`:** independently compare the change with the approved plan, acceptance criteria, tests, safety policy, and likely edge cases. Record findings with [the review template](../../templates/review.md). Classify them as `blocker`, `major`, `minor`, or `question`.
5. **Fix and re-review — `gateway:implementer`, `gateway:fix-reviewer`:** fix each blocker or major finding, rerun focused and required project checks, then independently verify the original concern and surrounding behavior. Repeat until blockers and major findings are resolved. Defer a security finding only with explicit user approval and a recorded rationale.
6. **Complete — `gateway:orchestrator`:** report completed tasks, verification evidence, findings/resolutions, known limitations, and follow-ups. Do not claim success without evidence.

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
