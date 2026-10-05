# Orchestration workflow

The orchestrator, implementor, reviewer, and tester form an optional project execution workflow. Install or supply all four skills to use the complete loop.

- **Orchestrator:** read user-supplied requirements and progress, choose tasks, delegate work, and own acceptance and progress updates.
- **Implementor:** complete one bounded task, preferably using a configured cheaper model.
- **Reviewer:** independently inspect the candidate against requirements and conventions.
- **Tester:** independently verify behavior and acceptance criteria.

Review or test findings return to implementation. The orchestrator accepts work only after both gates pass for the current candidate, and checks integrated milestones before advancing. Checkpoints support resumption after interruption.

Example request:

> Use the orchestrator skill to implement this project from `docs/prd.md`, using `progress.txt` for durable state. Use a cheaper implementor model supported by this host, independent reviewers and testers, and continue through review and repair until the milestone passes. Work within this repository.

Supply a model mapping or cost budget when those choices matter. Delegation requires agent spawning, result collection, and access to shared or integrated artifacts. A skill alone does not provide a background process or automatic restart.

See [host setup](orchestration-hosts.md) for Codex and Claude Code notes, [the coordination protocol](../skills/orchestrator/references/protocol.md) for state and acceptance rules, and [behavioral scenarios](../evals/orchestration.md) for evaluation.
