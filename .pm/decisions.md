# Decisions

## 2026-05-07: Builder Mode

Harness product discovery is running in Builder mode because the first version is a personal dogfood tool.

## 2026-05-07: External Demand Waived

External demand validation is waived for v0. The first proof is Harness improving Harness itself through safe long-running autonomy.

## 2026-05-07: MVP Requires `/goal` Long Run

The MVP is not a single-turn demo. It must support `/goal` long-running autonomous project advancement.

## 2026-05-07: Safety Validation Before Unbounded Mode

The first executable loop should use sync OpenCode delegation with `max_iterations: 5`. Unbounded `/goal` is allowed only after bounded validation passes.

## 2026-05-07: No Visual UI For MVP

The first version does not need a visual UI or mobile UX. The product surface is the coding agent, CLI, git, and `.pm` files.

## 2026-05-07: Supervisor Owns Branches, Intern Owns Commits

Supervisor manages branch policy. Intern creates a clear commit for each accepted task. Supervisor verifies commits before continuing.

## 2026-05-07: Slash-Command Delegation

Supervisor delegates to the intern via the `/harness-intern` slash command through the OpenCode CLI, not via `--agent harness-intern` flag or natural-language role-play ("Act as harness-intern"). The slash command ensures the full skill definition is loaded with correct routing, guardrails, and context. The canonical invocation is:

```bash
opencode run "/harness-intern Read and execute .pm/runtime/next-task.md exactly. Write .pm/runtime/worker-report.md and create one git commit for your task changes only." --file .pm/runtime/next-task.md
```

## 2026-05-07: Independent Review Agents

For branch+ risk or material worker claims, the supervisor must run an independent review via `/harness review ...` rather than trusting the worker's self-reported results. When review questions are separable (e.g., scope compliance vs test coverage vs security), the supervisor should spawn multiple independent review agents in parallel rather than running sequential reviews.

## 2026-05-07: Commit Taxonomy

Git commits in the PM loop fall into three categories:
- **Product commits**: Implementation changes created by the intern for a specific task. One per accepted task.
- **PM ledger commits**: Updates to `.pm/runtime/` state files (state.yaml, loop-log.md, handoff.md, acceptance-review.md). Created by the supervisor between iterations.
- **Checkpoint commits**: Point-in-time snapshots of the full `.pm/` tree, created explicitly by the supervisor or on user request for audit/recovery purposes.

Each commit type should have a clear message prefix: `[product]`, `[pm-ledger]`, or `[checkpoint]`.
