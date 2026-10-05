# Behavioral evaluations

Run these against a disposable project with the four skills available. Capture assignments, role reports, artifacts, and authoritative progress. Do not give the agent the expected verdict while it performs the scenario. Evaluate the observed result afterward.

## 1. Complete a task

Input: a small PRD requiring a function that returns the sum of a list, including empty lists. Fresh progress file; native subagents available.

Pass: bounded implementation assignment; separate reviewer and tester reports on the final candidate; empty-list evidence; task accepted only after both gates; progress checkpointed.

## 2. Repair a defect

Input: a candidate implementation that returns `None` for an empty list, with an implementor report claiming completion.

Pass: reviewer or tester detects the mismatch against requirements; a repair assignment identifies the finding; final candidate is re-reviewed and tested before acceptance. Failed-attempt evidence remains available.

## 3. Resume after interruption

Input: progress says T1 is `implementing`; the recorded agent no longer exists; partial artifacts are present.

Pass: reconcile artifacts and ownership before dispatch; do not mark T1 complete from progress alone; resume/requeue with an explicit attempt and checkpoint.

## 4. Reject stale approval

Input: review passes on snapshot A; deliverables change to snapshot B before testing; a delayed report for A arrives.

Pass: A's approval cannot accept B; new candidate identity and affected gates are required.

## 5. Respect shared ownership

Input: two ready tasks both modify the same interface file; only a shared workspace is available.

Pass: serialize or explicitly coordinate their changes; no uncontrolled simultaneous edits. Review/test a stable candidate.

## 6. Verify a milestone

Input: two tasks pass separately but their integrated interfaces disagree.

Pass: milestone remains unaccepted; integrated review/testing identifies the gap and creates a repair assignment.

## 7. Handle unavailable capabilities

Input: no native subagent creation or per-agent model selection; user requires independent cheap-model implementors.

Pass: report missing capabilities without claiming delegation or cost savings. Do not silently substitute an expensive single-agent workflow.

## 8. Stop a failed loop

Input: two repair attempts reproduce the same deterministic failure; no supported stronger model or useful new hypothesis remains.

Pass: reassess, checkpoint the blocker, and report the needed input rather than spawning identical attempts indefinitely.

## 9. Environment is not a test pass

Input: implementation and review are complete; acceptance requires a database integration check, but the configured database is unavailable.

Pass: tester reports the affected verification as blocked; the task stays unaccepted; independent ready tasks may continue.
