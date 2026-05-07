# Worker Task Packet

## Objective

Add a read-only branch correction plan helper so supervisor can recover from branch drift without guessing or mutating git state.

## Stage context

Stage 2: Unbounded `/goal` Dogfood. Dogfood already exposed a real branch drift case: work landed on `main` while the goal branch was `codex/dogfood`. Branch-policy validation now blocks delegation, but it does not yet tell the supervisor whether safe correction is possible or what exact non-destructive command sequence should be used.

## Read first

- `.pm/stable/roadmap.md`
- `.pm/runtime/state.yaml`
- `.pm/runtime/active-stage.md`
- `.pm/runtime/acceptance-review.md`
- `scripts/harness_runtime/pm_runtime.py`
- `scripts/harness_runtime/cli.py`
- `tests/test_pm_runtime.py`
- `Makefile`

## Task

Implement a read-only branch correction plan:

1. Add a helper in `scripts/harness_runtime/pm_runtime.py`, for example `get_branch_correction_plan(project_root: Path) -> dict`.
2. The helper must never mutate git state. It should inspect:
   - current branch
   - expected goal branch from `.pm/runtime/state.yaml`
   - whether the expected branch exists
   - whether the expected branch is an ancestor of the current branch
   - whether the current branch is an ancestor of the expected branch
3. Return one of these statuses:
   - `ok`: already on expected branch or no goal branch configured
   - `safe_fast_forward_goal_branch`: current branch contains expected branch history, so supervisor may safely fast-forward the goal branch to current HEAD and switch back
   - `safe_switch_to_goal_branch`: expected branch contains current branch history, so supervisor may safely switch to expected branch
   - `manual_review_required`: branches diverged or git state cannot prove safe correction
   - `unknown`: git/state inspection failed
4. Include explicit `commands` suggestions for the safe cases, but do not execute them.
5. Add CLI command `harness pm-branch-plan --project ...` that prints the plan clearly.
6. Add a Makefile target `pm-branch-plan`.
7. Add focused tests using mocks/subprocess patching. Do not require creating real git repositories unless that is simpler and deterministic.

## Allowed scope

- `scripts/harness_runtime/pm_runtime.py`
- `scripts/harness_runtime/cli.py`
- `tests/test_pm_runtime.py`
- `Makefile`
- `.pm/runtime/worker-report.md` only to write the final worker report

## Forbidden scope

- `scripts/harness_runtime/verify.py` because it has pre-existing uncommitted changes outside this task
- `subskills/opencode-cli/SKILL.md` because it has pre-existing uncommitted changes outside this task
- `subskills/opencode-cli/references/patterns.md` because it has pre-existing uncommitted changes outside this task
- `.pm/stable/*`
- Any command that mutates git state (`git switch`, `git branch -f`, `git merge`, `git reset`, `git checkout`, `git commit`, etc.) except the final task commit
- Product positioning or MVP boundary changes

## Acceptance criteria

- [ ] `get_branch_correction_plan()` is read-only and deterministic.
- [ ] Current branch equals expected branch returns `ok`.
- [ ] Missing goal branch returns `ok` with no commands.
- [ ] Expected branch ancestor of current branch returns `safe_fast_forward_goal_branch` with suggested commands.
- [ ] Current branch ancestor of expected branch returns `safe_switch_to_goal_branch` with suggested commands.
- [ ] Diverged branches return `manual_review_required` with no mutation commands executed.
- [ ] `harness pm-branch-plan --project /Users/qiujingyi.7/Harness` runs.
- [ ] `make pm-branch-plan` runs.
- [ ] `make verify` passes.
- [ ] One clear git commit is created for this task only.

## Required Harness process

Risk classify as branch: this touches PM runtime logic, CLI, tests, and Makefile, but must remain read-only. Use TDD-style implementation where practical: tests for branch plan behavior, then implementation, then verification, then report.

## Required verification commands

```bash
make test
make verify-ai
make pm-status
make pm-next
make pm-resume
make pm-branch-plan
make verify
git status --short
git log --oneline -1
```

## Required report file

`.pm/runtime/worker-report.md`

## If blocked

Write a blocker report to `.pm/runtime/blockers.md`. Do not invent product direction or change scope silently. Report exactly what is blocking and why.
