# User Journeys

## Primary Journey: Start A Long-Running Goal

### Context

The user is in their preferred coding agent and has a vague but serious product goal. Harness is a skill/router, so there is no separate app to open.

### Flow

1. User writes a natural-language goal or asks for `/goal` long-running project advancement.
2. Harness activates through the coding-agent skill system.
3. If product definition is incomplete, Harness runs product discovery and writes `.pm/stable`.
4. Harness initializes `.pm/runtime` state and safety settings.
5. Supervisor creates or confirms a goal branch.
6. Supervisor writes a bounded `next-task.md`.
7. Supervisor delegates to OpenCode intern in sync mode.
8. Intern executes the task, verifies, commits, and writes `worker-report.md`.
9. Supervisor reviews report, evidence, git status, commit hash, and scope.
10. Supervisor updates `acceptance-review.md`, `state.yaml`, `loop-log.md`, `handoff.md`, and `loop-control`.
11. Loop continues until stop condition, stage exit, or user interruption.

### Aha Moment 1

The user sees an agent-operated PM system: product contract, task packet, OpenCode worker, disciplined implementation, report, review, state update, and next iteration.

### Aha Moment 2

The user returns after time away and sees a high-completion product with git commits, test evidence, decisions, and a clear next step.

## Return Journey

### User Goal

Understand what happened while away and decide whether to continue.

### Required Summary

- Current branch
- Iterations completed
- Commits created
- Tests and verification commands run
- Accepted and rejected worker reports
- Stop reason if any
- Remaining risks
- Recommended next action

## Recovery Journey

### Trigger

The agent session, worker process, or user attention is interrupted.

### Resume Protocol

1. Read `.pm/runtime/handoff.md`.
2. Read `.pm/runtime/state.yaml`.
3. Read the last entries in `.pm/runtime/loop-log.md`.
4. Inspect `.pm/runtime/loop-control`.
5. Continue from the next expected action, not from memory.

## Failure Journey

### Trigger

Worker report is missing, tests fail, scope is violated, evidence is weak, git state is dirty, or risk escalates.

### Recovery

Harness records the failure, updates failure tracking, stops or requests rework, and asks for user decision when needed.
