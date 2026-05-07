# Acceptance Review

## Verdict

Rejected: needs rework.

## Reviewed Worker Commit

- Commit: `6125f40`
- Branch: `codex/dogfood`
- Worker report: `.pm/runtime/worker-report.md`

## Evidence Reviewed

- `uv run harness pm-status --project /Users/qiujingyi.7/Harness`: passed
- `uv run python -m unittest discover -s tests`: passed, 18 tests
- `uv run harness verify-ai --project /Users/qiujingyi.7/Harness`: passed, 47 passed / 0 failed / 1 warning
- `git diff --name-only 0232472..6125f40`: only `cli.py`, `pm_runtime.py`, and tests
- `scripts/harness_runtime/verify.py`: still has pre-existing user change only

## Rejection Reason

The implementation does not match the supervisor protocol for loop-control values.

`scripts/harness_runtime/pm_runtime.py` currently accepts only:

- `CONTINUE`
- `STOP`
- `USER_DECISION`

But the supervisor protocol and `.pm/runtime` contract use:

- `CONTINUE`
- `STOP`
- `NEEDS_USER_DECISION`
- `BLOCKED`
- `STAGE_EXIT_REACHED`

This would cause valid supervisor stop states to be reported as invalid. That is a correctness bug in the first PM runtime slice.

## Required Rework

Update the PM runtime loop-control classifier and tests so it accepts and describes the protocol values:

- `CONTINUE`
- `STOP`
- `NEEDS_USER_DECISION`
- `BLOCKED`
- `STAGE_EXIT_REACHED`

Do not keep `USER_DECISION` unless there is a compatibility reason; if kept, it must be clearly treated as legacy alias and tested.

## Scope For Rework

Allowed:

- `scripts/harness_runtime/pm_runtime.py`
- `tests/test_pm_runtime.py`
- `.pm/runtime/worker-report.md`

Forbidden:

- `scripts/harness_runtime/verify.py`
- `.pm/stable/*`
- Product boundary changes
- New CLI features beyond fixing loop-control semantics

## Next Action

Request worker rework with the above correction.
