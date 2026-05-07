# Product Contract

## Product

Harness is a governed PM layer for AI coding agents. A user opens their normal coding agent, gives a vague product goal, and Harness turns that goal into a frozen product contract, then supervises long-running implementation through disciplined worker agents.

## Mode

- Project type: Builder
- First release level: dogfood
- First dogfood target: Harness itself
- Primary interface: coding-agent skill and CLI/runtime files, not a standalone app

## Problem

AI coding agents can complete bounded tasks, but long-running autonomous product delivery drifts without a product contract, PM discipline, state tracking, git boundaries, and independent evidence review. The user wants to leave the desk while agents keep advancing the project without creating an unreviewable mess.

## Target User

The first user is a solo builder or independent developer who already uses coding agents such as Codex, Claude Code, or OpenCode, has long-running personal or production projects, and wants agent work to continue while preserving control, reversibility, and maintainability.

## Not First Users

- One-off vibe coding demos
- Short-lived toy projects
- Enterprise approval workflows
- Multi-user permission systems
- Cloud project-management platforms
- Automatic deployment or release workflows

Non-technical users are part of the long-term vision, but the first dogfood release optimizes for a technical solo builder because Harness itself must be improved through real use.

## Core Use Case

The user enters a vague product goal in their preferred coding agent. Harness activates, clarifies the product definition, freezes `.pm/stable`, starts a supervisor loop, delegates bounded tasks to OpenCode intern, reviews reports, updates runtime state, and continues through `/goal` until a stop condition requires user input.

## Job To Be Done

When I have a serious product idea or project goal, I want my coding agent to behave like a small autonomous company with a PM and disciplined engineers, so I can step away and return to a high-completion product with clear evidence, git history, and next steps.

## MVP Boundary

### Included

- Product definition through `grill-product`
- `.pm/stable` product contract
- `.pm/runtime` state, task, report, review, handoff, loop log, and control files
- A real `/goal` supervisor loop
- Sync OpenCode worker delegation as the first supported execution mode
- Safety validation with `max_iterations: 5` before unbounded goal mode
- Unbounded `/goal` mode after validation passes
- Supervisor-owned branch management
- Intern-owned per-task commits
- Supervisor review of git status, commit hash, scope, tests, and report evidence
- Stop conditions for product boundary changes, core/infra/security/deployment risk, repeated failures, missing evidence, and stage exit
- Resume from `handoff.md`, `state.yaml`, and recent `loop-log.md`

### Excluded

- Visual dashboard
- Mobile UI
- Multiple parallel workers
- Cross-repository portfolio management
- Background daemon or queue system
- Enterprise permissions or approval flows
- Automatic merge, push, publish, or deploy
- Supervisor changing product positioning, MVP boundary, or core tech stack without user approval

## Non-Goals

- Replace the user's preferred coding agent
- Become a generic project-management app
- Hide execution behind a black-box automation layer
- Optimize for impressive demos at the expense of reversibility

## Success Criteria

- Harness can run at least 5 consecutive supervisor iterations on Harness itself.
- At least 4 out of 5 iterations produce valid `next-task.md`, `worker-report.md`, `acceptance-review.md`, state updates, and git commits.
- The loop can resume after interruption without guessing.
- The supervisor stops correctly on risk, boundary, repeated-failure, missing-evidence, or stage-exit conditions.
- The user can return after a run and understand what changed, why, what evidence exists, and what remains.

## Open Questions

- Exact CLI command shape for starting `/goal` long-run mode.
- Whether branch creation should happen per goal, per stage, or per high-risk checkpoint.
- Whether independent verification should be handled by supervisor directly or delegated to a separate reviewer worker in later versions.
