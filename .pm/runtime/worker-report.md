# Worker Report

## Task summary

Add a deterministic read-only PM next-action decision helper (`decide_next_action`) and `harness pm-next` CLI command so the supervisor can compute whether to continue, review, request rework, stop, or ask the user before delegating.

## What was done

- **Modified**: `scripts/harness_runtime/pm_runtime.py`
  - Added `decide_next_action(project_root: Path) -> dict`: a read-only decision helper that reuses `get_pm_status()` and inspects state, loop-control, worker-report status, iteration limits, and failure counters
  - Implements all 10 decision rules from the task spec: invalid state → stop, STOP → stop, NEEDS_USER_DECISION → request_user_decision, BLOCKED → blocked, STAGE_EXIT_REACHED → stop, max_iterations reached → stop, consecutive_failures exceeded → request_user_decision, waiting_for_worker without report → blocked, invalid report → request_rework, valid report → review, CONTINUE → delegate
  - Returns dict with `action`, `reason`, and `details` keys

- **Modified**: `scripts/harness_runtime/cli.py`
  - Added `harness pm-next --project <path>` CLI command that prints action, reason, and details
  - Command is read-only and does not mutate files

- **Modified**: `tests/test_pm_runtime.py`
  - Added `TestDecideNextAction` class with 14 tests covering all decision paths
  - Added `_make_project` helper for building tempfile fixtures with configurable state, loop-control, and worker report
  - Tests cover: invalid runtime, STOP, NEEDS_USER_DECISION, BLOCKED, STAGE_EXIT_REACHED, max_iterations reached, max_iterations null allows continue, consecutive failures exceeded, waiting without report, waiting with placeholder, invalid report, valid report + waiting_for_worker, valid report + review_pending, ready_to_delegate + CONTINUE
  - Total tests: 39 (was 25, net +14)

## Changed files

- `scripts/harness_runtime/pm_runtime.py`
- `scripts/harness_runtime/cli.py`
- `tests/test_pm_runtime.py`

## Commands run

```
$ uv run harness pm-next --project /Users/qiujingyi.7/Harness
=== PM Next Action ===
Action: review
Reason: worker_report_valid_ready_for_review
Details:
  - phase=waiting_for_worker, report=valid

$ uv run harness pm-status --project /Users/qiujingyi.7/Harness
=== PM Runtime Status ===
Structure: OK
Stage: feasibility
Phase: waiting_for_worker
Loop iteration: 2
Worker report: valid — 10 section(s) found
✅ PM runtime state is valid.

$ uv run python -m unittest discover -s tests
Ran 39 tests in 0.277s
OK

$ uv run harness verify-ai --project /Users/qiujingyi.7/Harness
47 passed, 0 failed, 1 warnings
🎉 All required checks passed.
```

## Test results

39 tests pass (was 25). 14 new tests added:

- `test_invalid_pm_runtime_returns_stop`: missing state → stop
- `test_loop_control_stop`: STOP directive → stop
- `test_loop_control_needs_user_decision`: NEEDS_USER_DECISION → request_user_decision
- `test_loop_control_blocked`: BLOCKED → blocked
- `test_loop_control_stage_exit_reached`: STAGE_EXIT_REACHED → stop
- `test_max_iterations_reached`: loop_iteration >= max_iterations → stop
- `test_max_iterations_null_allows_continue`: null max_iterations → delegate
- `test_consecutive_failures_exceeded`: failures >= max → request_user_decision
- `test_waiting_for_worker_no_report_returns_blocked`: no report → blocked
- `test_waiting_for_worker_placeholder_report_returns_blocked`: placeholder → blocked
- `test_invalid_worker_report_returns_request_rework`: invalid sections → request_rework
- `test_valid_report_waiting_for_worker_returns_review`: valid + waiting → review
- `test_valid_report_review_pending_returns_review`: valid + review_pending → review
- `test_ready_to_delegate_with_continue_returns_delegate`: CONTINUE → delegate

## Acceptance criteria

- [x] `harness pm-next --project /Users/qiujingyi.7/Harness` runs and returns a deterministic next action
- [x] Decision helper has tests for stop, user decision, blocked, request rework, review, and delegate paths
- [x] Decision helper respects `max_iterations` and `consecutive_failures`
- [x] The helper is read-only and does not mutate `.pm` or git
- [x] `uv run python -m unittest discover -s tests` passes (39/39)
- [x] `uv run harness verify-ai --project /Users/qiujingyi.7/Harness` still passes (47/47)
- [x] A clear git commit `9265b88` for this task only (verify.py excluded)

## Problems encountered

None.

## Deviations

None. All changes stayed within allowed scope. `verify.py` was not touched. No full loop execution, OpenCode invocation, file mutation, branch mutation, auto-merge, push, deploy, or stage advancement was implemented.

## Evidence

- Commit: `9265b88` on branch `main`
- Pre-existing dirty file `verify.py` intentionally excluded from commit
- `git status --short` shows only `M scripts/harness_runtime/verify.py` as remaining dirty file
