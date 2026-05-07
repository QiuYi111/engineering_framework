# Worker Report

## Task summary

Add PM runtime branch-policy validation so `pm-status` and `pm-next` detect when the current git branch does not match the supervisor-managed goal branch and block delegation before worker execution.

## What was done

- **Modified**: `scripts/harness_runtime/pm_runtime.py`
  - Added `validate_branch_policy(project_root: Path) -> dict`: compares `inspect_git()` branch with `state.git.current_goal_branch`
  - Returns status `ok` (match or no goal branch set), `mismatch` (wrong branch), or `unknown` (git error)
  - Exposed `branch_policy` in `get_pm_status()` output dict
  - Updated `decide_next_action()` to check branch policy early and return `request_user_decision` on mismatch, before loop-control and delegation logic

- **Modified**: `scripts/harness_runtime/cli.py`
  - Updated `harness pm-status` to display branch-policy status with icon (✅/❌/⚠️), current branch, expected branch, and reason

- **Modified**: `tests/test_pm_runtime.py`
  - Added `TestValidateBranchPolicy` class with 6 tests: matching branch, mismatched branch, no goal branch, git error, branch_policy in get_pm_status, branch match allows normal flow
  - Added 4 branch-policy tests in `TestDecideNextAction`: mismatch returns request_user_decision, match allows delegate, missing goal branch does not block
  - Total tests: 47 (was 39, net +8)

## Changed files

- `scripts/harness_runtime/pm_runtime.py`
- `scripts/harness_runtime/cli.py`
- `tests/test_pm_runtime.py`

## Commands run

```
$ uv run harness pm-status --project /Users/qiujingyi.7/Harness
=== PM Runtime Status ===
Structure: OK
Stage: feasibility
Phase: waiting_for_worker
Loop iteration: 3
...
Branch policy: ✅ ok
  Current: codex/dogfood
  Expected: codex/dogfood
  on expected branch 'codex/dogfood'
✅ PM runtime state is valid.

$ uv run harness pm-next --project /Users/qiujingyi.7/Harness
=== PM Next Action ===
Action: review
Reason: worker_report_valid_ready_for_review
Details:
  - phase=waiting_for_worker, report=valid

$ uv run python -m unittest discover -s tests
Ran 47 tests in 0.577s
OK

$ uv run harness verify-ai --project /Users/qiujingyi.7/Harness
47 passed, 0 failed, 1 warnings
🎉 All required checks passed.
```

## Test results

47 tests pass (was 39). 8 new tests added:

- `test_matching_branch_returns_ok`: on expected branch → status ok
- `test_mismatched_branch_returns_mismatch`: wrong branch → status mismatch
- `test_no_goal_branch_returns_ok`: no goal branch set → status ok (not enforced)
- `test_git_error_returns_unknown`: git unavailable → status unknown
- `test_branch_policy_in_get_pm_status`: branch_policy key present in status output
- `test_branch_mismatch_returns_request_user_decision`: pm-next blocks on mismatch
- `test_branch_match_allows_normal_flow`: pm-next delegates when on correct branch
- `test_missing_goal_branch_does_not_block`: pm-next delegates when no goal branch

## Acceptance criteria

- [x] Branch mismatch is visible in `harness pm-status`
- [x] Branch mismatch causes `harness pm-next` to avoid `delegate` and `review`
- [x] Matching branch allows normal decision flow
- [x] Missing expected goal branch does not block
- [x] `uv run python -m unittest discover -s tests` passes (47/47)
- [x] `uv run harness verify-ai --project /Users/qiujingyi.7/Harness` still passes (47/0/1)
- [x] A clear git commit for this task only (verify.py excluded)

## Problems encountered

None.

## Deviations

None. All changes stayed within allowed scope (`pm_runtime.py`, `cli.py`, `test_pm_runtime.py`). `verify.py` was not touched. `.pm/stable/*` was not modified. No git branch mutation, auto-merge, auto-push, deployment, auth, payment, or security features were implemented.

## Evidence

- Branch: `codex/dogfood` (verified on correct branch before commit)
- `pm-status` shows `Branch policy: ✅ ok` with current=expected=`codex/dogfood`
- `pm-next` returns `review` (normal flow, not blocked)
- Pre-existing dirty file `verify.py` intentionally excluded from commit
