# Coordination protocol

## State ownership

The orchestrator is the sole writer of authoritative progress. Children return reports; they do not mark tasks accepted. Another orchestrator must not take ownership while the previous one is active. Record the run identifier and reconcile ownership on resume.

Preserve the user's progress format. Track these logical fields even if stored as prose: task ID, milestone, requirement references, dependencies, criteria, status, attempt, owner/live agent ID, owned paths, candidate identity, report/evidence locations, findings, repair count, and next action.

## Transitions

`pending → ready → implementing → implemented → reviewing → testing → accepted`

- `ready`: dependencies are accepted and required inputs exist.
- `implemented`: a deliverable exists with a report; this is not acceptance.
- Review `changes_required` or test `fail`: return to `implementing` with findings and a new attempt.
- Missing essential input/environment: `blocked`, with reason and resumption condition; continue independent tasks.
- Material edits after approval: reopen the affected review/test gates.
- Interrupted owner: reconcile artifacts, then resume the attempt or requeue it. Never assume success or failure from silence alone.

If an interrupted worker left a usable candidate without a matching implementation report, the orchestrator may adopt it for review after recording a reconciliation report: source attempt, owned files, new candidate identity, observed changes, and missing validation. This establishes an `implemented` candidate for independent review/testing, not acceptance. Do not recreate already-present work merely to obtain a worker report.

Statuses describe coordination, not proof. Acceptance requires matching reviewer and tester passes for the current candidate, plus coverage of all criteria. Unverified criteria remain open; changing a criterion requires alignment with the user's requirements.

## Candidate identity and integration

Use a commit ID for a clean committed candidate, or a snapshot ID tied to recorded file contents/hashes for uncommitted work. A branch name or directory path alone is insufficient. Include relevant untracked files in a snapshot. Freeze relevant artifacts during review/testing or invalidate reports when they change.

With isolated workspaces, integrate the candidate before final gates. Integration can change behavior, so reports on an earlier worker workspace cannot automatically approve the integrated result. For shared workspaces, serialize conflicting edits and track dependencies that may change under review.

## Dispatch

An assignment identifies the role skill to load, task and attempt, goal, sources, workspace, owned paths, criteria, dependency/interface state, required evidence, report destination, and applicable limits. Children receive a bounded task rather than the entire project's conversation.

Only dispatch within actual host concurrency limits. For a native multi-agent host, use its tools. For a separately configured runner, use its documented interface. Do not invent command names or assume that a standard chat has agent creation tools.

Configure roles as `orchestrator`, `implementor`, `reviewer`, and `tester`; map them to models supported by the host. Prefer a cheaper implementor by default and stronger orchestration/review where available. If per-agent model choice is unsupported, disclose inheritance and respect any strict user cost constraint. Do not silently substitute an expensive model for a required cheap one.

## Review and test gates

Give reviewers original requirements and actual artifacts, not only worker claims. Give testers the same candidate plus environment constraints. Preserve findings and evidence in progress or linked reports. Resolve disagreements by reproducing behavior and checking requirements; do not select whichever verdict permits advancing.

Reviewer: `pass | changes_required | blocked`.
Tester: `pass | fail | blocked`.
Implementor: `implemented | blocked`.
All reports identify the target, attempt, and candidate. Check those fields before applying a verdict; delayed results for an older attempt cannot accept newer work.

## Milestones and recovery

Milestone completion requires review and verification of integrated behavior against original milestone criteria. Create bounded repair tasks for uncovered gaps. A task list reaching 100% is not sufficient.

Checkpoint after dispatch, results, acceptance, integration, and stopping. On restart, reread source requirements, reconcile changed scope and candidate state, check live ownership, and reopen any invalidated gates. Preserve historical evidence instead of overwriting a failed attempt with a successful one.

Stop with explicit state and the next useful action. Budget exhaustion, cancellation, host session termination, and missing capabilities do not mean the project is complete. No retry limit grants new permissions for external writes or broader scope.
