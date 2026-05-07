# Worker Task Packet

## Objective

Replace supervisor delegation/review protocol with tested slash-command OpenCode invocations and add Makefile entrypoints for PM runtime verification.

## Stage context

Stage 2: Unbounded `/goal` Dogfood. Stage 1 proved bounded feasibility, but dogfood exposed protocol drift: supervisor docs and worker config still describe old natural-language or `--agent harness-intern` delegation. This task hardens the PM loop before longer autonomous runs.

## Read first

- `.pm/stable/product.md`
- `.pm/stable/ux-principles.md`
- `.pm/stable/user-journeys.md`
- `.pm/stable/ui-direction.md`
- `.pm/stable/roadmap.md`
- `.pm/runtime/active-stage.md`
- `.pm/decisions.md`
- `subskills/supervisor/references/loop-steps.md`
- `references/templates/pm/worker-config.yaml`

## Task

Implement these protocol corrections:

1. Update `.pm/runtime/worker-config.yaml` so sync delegation uses an explicit slash command:
   `opencode run "/harness-intern Read and execute .pm/runtime/next-task.md exactly. Write .pm/runtime/worker-report.md and create one git commit for your task changes only." --file .pm/runtime/next-task.md`
2. Update `references/templates/pm/worker-config.yaml` with the same slash-command pattern.
3. Update `subskills/supervisor/references/loop-steps.md` Step 5 so supervisor delegation uses `/harness-intern ...`, not natural-language role-play and not `--agent harness-intern`.
4. Update Step 6 review guidance to require independent OpenCode review via `/harness review ...` for branch+ risk or when worker claims are material. Recommend spawning multiple independent review agents when the review questions are separable.
5. Record the dogfood decisions in `.pm/decisions.md`: slash-command delegation, independent reviewer agents, and commit taxonomy separating product commits, PM ledger commits, and checkpoint commits.
6. Add a root `Makefile` because Harness project conventions require Makefile entrypoints. Include at least `test`, `verify-ai`, `pm-status`, `pm-next`, `pm-resume`, and `verify` targets. The targets should wrap the existing `uv run ...` commands instead of duplicating them throughout docs.

## Allowed scope

- `.pm/runtime/worker-config.yaml`
- `references/templates/pm/worker-config.yaml`
- `subskills/supervisor/references/loop-steps.md`
- `.pm/decisions.md`
- `Makefile`
- `.pm/runtime/worker-report.md` only to write the final worker report

## Forbidden scope

- Changing product positioning
- Expanding MVP boundary
- Changing core tech stack
- Core/infra/security/payment/auth changes without explicit approval
- `scripts/harness_runtime/verify.py` because it has pre-existing uncommitted changes outside this task
- `subskills/opencode-cli/SKILL.md` because it has pre-existing uncommitted changes outside this task
- `subskills/opencode-cli/references/patterns.md` because it has pre-existing uncommitted changes outside this task
- Any `.pm/stable/*` product-contract files

## Acceptance criteria

- [ ] Worker config and template use `/harness-intern ...` slash-command delegation.
- [ ] Supervisor loop docs no longer recommend `--agent harness-intern` or "Act as harness-intern" delegation.
- [ ] Supervisor review docs require independent `/harness review ...` review for branch+ or material claims and mention parallel independent reviewers for separable review questions.
- [ ] `.pm/decisions.md` records slash delegation, reviewer-agent, and commit-taxonomy decisions.
- [ ] Root Makefile exists and `make test`, `make verify-ai`, `make pm-status`, `make pm-next`, `make pm-resume`, and `make verify` work.
- [ ] Pre-existing dirty files listed in forbidden scope are not modified or staged by this task.
- [ ] One clear git commit is created for this task only.

## Required Harness process

Risk classify as leaf/branch. Use direct implementation plus report. Do not run broader architecture changes.

## Required verification commands

```bash
make test
make verify-ai
make pm-status
make pm-next
make pm-resume
make verify
git status --short
```

## Required report file

`.pm/runtime/worker-report.md`

## If blocked

Write a blocker report to `.pm/runtime/blockers.md`. Do not invent product direction or change scope silently. Report exactly what is blocking and why.
