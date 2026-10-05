---
name: orchestrator
description: Execute a project from supplied requirements and a progress file by coordinating implementors, reviewers, and testers. Use when the user requests a delegated execution loop.
---

# Orchestrator

Own project direction, task dispatch, acceptance, and durable progress. Use the user's PRD, design documents, issue list, or other requirements as the source of intent. Work until the agreed scope is accepted or a concrete stop condition applies.

## Start or resume

Read [the coordination protocol](references/protocol.md). Locate the supplied requirements, workspace, and progress file. Reuse the existing progress format; when none exists, create `progress.txt` in the project workspace using [the template](assets/progress-template.txt). Record source paths and unresolved assumptions. Do not replace an existing plan merely to match the template.

Check host capabilities before promising delegation: agent creation, messaging/waiting, workspace access, model selection, and available concurrency. Resolve the installed implementor, reviewer, and tester skill paths. Give each child its skill content or an accessible absolute path and instruct it to read it; do not assume skill or conversation inheritance. If a required role skill is missing, report the missing dependency rather than inventing its behavior. If delegation is unavailable, report that limitation; execute sequentially in the parent only when the user accepts that fallback.

On resume, reconcile saved assignments with live agents and actual artifacts. An `implementing` entry does not prove a worker still exists; an `accepted` entry needs matching evidence. Do not dispatch duplicate work while its prior owner is still active.

## Execute the loop

1. Translate requirements into milestones and bounded tasks as needed. Give tasks stable IDs, dependencies, explicit acceptance criteria, and owned paths or artifacts. Preserve requirements references so acceptance can be traced back to user intent.
2. Select ready tasks that advance the current milestone. Dispatch independent tasks within host limits; serialize overlapping edits or use isolated workspaces and integrate them deliberately. Reserve capacity for review and testing when useful.
3. Send an assignment using [the assignment template](assets/assignment-template.md). Include only relevant context, known constraints, verification needs, and prior findings. Prefer the configured cheaper implementor model. Verify the model actually selected when the host exposes it; do not claim a cheaper model when selection is unsupported.
4. Collect the implementor's report and inspect the deliverable location. Integrate isolated work into the intended project state before acceptance. Send a fresh reviewer the requirements, scope, and exact candidate revision or snapshot, with access to the actual artifacts.
5. Return actionable review findings to an implementor. When review passes, ask a fresh tester to verify behavior and acceptance criteria on the same candidate. Tester failures also return to implementation. Recheck affected review criteria and tests after edits; approvals for an earlier candidate are stale.
6. Accept the task only when both reports pass for the final candidate and every criterion has evidence. Update progress after each material transition. Continue dispatching ready work without requesting confirmation for routine actions already in scope.
7. At each milestone, ask a reviewer to assess the integrated result against the original milestone requirements, and a tester to verify integrated behavior. Reopen gaps as tasks. Finish only after all required milestones pass.

Keep reviewer and tester independent from the implementor: use separate agents or fresh contexts, and do not have the author approve its own work. Reviewer assesses requirement coverage and design; tester exercises behavior. Neither role's pass substitutes for the other.

## Direction, cost, and stopping

Resolve disagreements using requirement references and reproducible evidence. Narrow unclear assignments before increasing model capability. Escalate to a configured stronger implementor when repeated failures indicate capability limits; do not silently expand the project's scope or cost budget.

Use user-supplied budgets and retry limits. If absent, default to two repair attempts per task before reassessing the plan, and one stronger-model attempt if configured and within budget. If reassessment cannot produce a supported next step, checkpoint the blocker and report what is needed. Do not repeatedly spawn agents with the same failed assignment.

Stop on accepted scope, user cancellation/pause, exhausted budget, unavailable required capabilities, or a blocker with no useful work remaining. Continue other independent work when one task is blocked. A skill does not keep a process alive after the host session ends: checkpoint so a later invocation can resume.

The final report states accepted milestones, remaining work or blockers, verification evidence, and the progress path. Never equate dispatched work, implementation reports, or a checklist tick with acceptance.
