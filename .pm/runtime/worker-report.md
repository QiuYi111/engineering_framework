# Worker Report

## Task summary

Add a read-only PM loop run summary helper (`get_loop_summary`) and `pm-summary` CLI command for supervisor audit.

## What was done

- **Added**: `get_loop_summary()` in `scripts/harness_runtime/pm_runtime.py`
  - Parses loop-log.md entries to count accepted iterations, reworks, blockers
  - Extracts delivered iteration summaries (truncated to 120 chars)
  - Extracts last worker commit hash and date range
  - Computes valid_rate from state.yaml

- **Added**: `pm-summary` CLI command in `scripts/harness_runtime/cli.py`
  - Click command printing formatted audit summary
  - Follows same pattern as existing PM commands

- **Updated**: `Makefile`
  - Added `pm-summary` to `.PHONY`
  - Added `pm-summary` target with help text

- **Added**: 7 focused tests in `tests/test_pm_runtime.py`
  - `TestGetLoopSummary` class with all spec-required test cases

## Changed files

- `scripts/harness_runtime/pm_runtime.py` — added `get_loop_summary()` function
- `scripts/harness_runtime/cli.py` — added import and `pm-summary` command
- `Makefile` — added `.PHONY` entry and target
- `tests/test_pm_runtime.py` — added `TestGetLoopSummary` with 7 tests

## Commands run

```
$ make test
72 passed in 0.70s
EXIT: 0

$ make verify-ai
47 passed, 0 failed, 1 warnings
EXIT: 0

$ make pm-status
✅ PM runtime state is valid.
Branch policy: ✅ ok
EXIT: 0

$ make pm-summary
=== PM Loop Run Summary ===
Stage: delivery
Duration: 2026-05-07
Last commit: 659c499
Consecutive failures: 0
Blockers: 0
Iterations: 7 accepted, 1 reworks
Delivered (7):
  1. PM runtime health-check foundation and `harness pm-status` are available
  ...
EXIT: 0

$ make verify
✅ PM runtime state is valid.
EXIT: 0

$ git status --short
(none)
```

## Test results

72 tests pass (7 new tests in `TestGetLoopSummary`). All pre-existing tests continue to pass.

New tests:
- `test_empty_log_returns_zeros` — no loop-log → all zeros
- `test_single_accepted_iteration` — one accepted entry → total_iterations=1
- `test_rework_counted` — one needs_rework entry → total_reworks=1
- `test_valid_rate_from_state` — state with 5/5 → valid_rate=1.0
- `test_dates_extracted` — two entries with different dates → range
- `test_last_commit_extracted` — Worker commit → last_commit populated
- `test_blockers_counted` — Phase: blocked → blockers=1

## Acceptance criteria

- [x] `get_loop_summary()` is read-only and deterministic
- [x] Correctly counts accepted iterations from loop-log.md
- [x] Correctly counts rework entries
- [x] Correctly computes valid_rate from state.yaml
- [x] Extracts last worker commit hash
- [x] Extracts date range from loop-log
- [x] Extracts accepted-result summaries (truncated to 120 chars)
- [x] `harness pm-summary` runs and prints formatted output
- [x] `make pm-summary` runs
- [x] `make verify` passes
- [x] One clear git commit created (`ceec0ce`)

## Problems encountered

None.

## Deviations

None. All changes stayed within allowed scope.

## Evidence

- Branch: `codex/dogfood` (branch-policy ok)
- 72/72 tests pass (7 new + 65 existing)
- `make pm-summary` shows 7 accepted iterations with correct delivered summaries
- Forbidden files not touched
