# Worker Report

## Task summary

Strengthen `validate_worker_report()` so it validates required worker-report sections, returning `invalid` with missing section details for incomplete reports instead of accepting any report that contains markdown headings.

## What was done

- **Modified**: `scripts/harness_runtime/pm_runtime.py`
  - Added `REQUIRED_REPORT_SECTIONS`: 9 required sections with canonical names and acceptable alternative heading texts (e.g., "Task summary" accepts headings "task summary" or "objective")
  - Added `_section_present()` helper for case-insensitive substring matching
  - Rewrote `validate_worker_report()` to check all required sections after the existing placeholder/not_started detection
  - New return status `invalid` with `missing_sections` list when required sections are absent
  - Added `missing_sections` key (empty list) to all return paths for consistent dict shape

- **Modified**: `scripts/harness_runtime/cli.py`
  - `pm-status` command now prints missing sections below the worker report status line when present

- **Modified**: `tests/test_pm_runtime.py`
  - Replaced the 4 original `TestValidateWorkerReport` tests with 8 tests covering all status transitions
  - New tests: `test_valid_report_with_all_sections`, `test_invalid_report_missing_sections`, `test_alternative_heading_names_accepted`, `test_no_headings_is_not_started`, `test_invalid_report_missing_sections_field_populated`
  - Total tests: 25 (was 21, net +4)

## Changed files

- `scripts/harness_runtime/pm_runtime.py`
- `scripts/harness_runtime/cli.py`
- `tests/test_pm_runtime.py`

## Commands run

```
$ uv run harness pm-status --project /Users/qiujingyi.7/Harness
Worker report: invalid — missing required sections: Changed files, Test results, Problems encountered
  Missing: Changed files, Test results, Problems encountered
✅ PM runtime state is valid.

$ uv run python -m unittest discover -s tests
Ran 25 tests in 0.045s
OK

$ uv run harness verify-ai --project /Users/qiujingyi.7/Harness
47 passed, 0 failed, 1 warnings
🎉 All required checks passed.
```

## Test results

25 tests pass (was 21). New test cases:

- `test_valid_report_with_all_sections`: complete report → `valid`
- `test_invalid_report_missing_sections`: partial report → `invalid` with 7 missing sections listed
- `test_alternative_heading_names_accepted`: alternative names like "Objective", "Files Changed", "Verification" → `valid`
- `test_no_headings_is_not_started`: plain text → `not_started`
- `test_invalid_report_missing_sections_field_populated`: sparse report → `invalid` with populated `missing_sections`

## Acceptance criteria

- [x] `validate_worker_report()` returns `invalid` with missing section details for incomplete reports
- [x] Placeholder reports are still not treated as valid
- [x] A complete report with required sections returns `valid`
- [x] `harness pm-status` still runs successfully on this repository
- [x] `uv run python -m unittest discover -s tests` passes (25/25)
- [x] `uv run harness verify-ai --project /Users/qiujingyi.7/Harness` passes (47/47)
- [x] Clear git commit `8cfc1e6` for this task only (verify.py excluded)

## Problems encountered

None.

## Deviations

None. All changes stayed within allowed scope. `verify.py` was not touched.

## Evidence

- Commit: `8cfc1e6` on branch `codex/dogfood`
- Pre-existing dirty file `verify.py` intentionally excluded from commit
- `git status --short` shows only `M scripts/harness_runtime/verify.py` as remaining dirty file
