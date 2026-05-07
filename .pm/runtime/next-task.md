# Worker Task Packet

## Objective

Add a read-only PM loop run summary helper (`get_loop_summary`) that produces a concise audit summary of the entire supervisor run from `loop-log.md` and `state.yaml`.

## Stage context

Stage 2: Unbounded `/goal` Dogfood. The roadmap Stage 3 exit criteria require "the user can audit the full run from git and `.pm/runtime`." Currently, auditing requires manually reading `loop-log.md` (175 lines and growing). A structured summary command would make auditing instant.

## Read first

- `.pm/runtime/state.yaml`
- `.pm/runtime/loop-log.md`
- `scripts/harness_runtime/pm_runtime.py`
- `scripts/harness_runtime/cli.py`
- `tests/test_pm_runtime.py`
- `Makefile`

## Task

Implement a read-only loop run summary:

1. Add a helper in `scripts/harness_runtime/pm_runtime.py`, for example `get_loop_summary(project_root: Path) -> dict`.
2. The helper must parse `loop-log.md` entries and `state.yaml` to produce:
   - `total_iterations`: count of accepted iterations (entries with "Verdict: accepted")
   - `total_reworks`: count of rework entries (entries with "Phase: needs_rework")
   - `valid_rate`: ratio of `iteration_valid_count` / `iteration_total_count` from state.yaml
   - `consecutive_failures`: current value from state.yaml
   - `stage`: current stage from state.yaml
   - `last_commit`: the most recent worker commit hash found in loop-log
   - `duration_note`: first log entry date and last log entry date (e.g., "2026-05-07 → 2026-05-07")
   - `delivered`: list of accepted iteration summaries — for each accepted review entry, extract the "Accepted result:" line (first 120 chars)
   - `blockers`: count of entries with "Phase: blocked" or "action: blocked"
3. Add CLI command `harness pm-summary --project ...` that prints the summary in a clear, formatted way (not raw JSON).
4. Add a Makefile target `pm-summary`.
5. Add focused tests. Use the existing `_parse_loop_log_entries` helper. Test with a small synthetic loop-log string written to a temp file, not the real one.

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
- Any command that mutates git state except the final task commit
- Product positioning or MVP boundary changes

## Acceptance criteria

- [ ] `get_loop_summary()` is read-only and deterministic.
- [ ] Correctly counts accepted iterations from loop-log.md.
- [ ] Correctly counts rework entries.
- [ ] Correctly computes valid_rate from state.yaml.
- [ ] Extracts last worker commit hash.
- [ ] Extracts date range from loop-log.
- [ ] Extracts accepted-result summaries (truncated to 120 chars).
- [ ] `harness pm-summary --project /Users/qiujingyi.7/Harness` runs and prints formatted output.
- [ ] `make pm-summary` runs.
- [ ] `make verify` passes.
- [ ] One clear git commit is created for this task only.

## Required Harness process

Risk classify as branch: this touches PM runtime logic, CLI, tests, and Makefile. Use TDD-style implementation where practical.

## Required verification commands

```bash
make test
make verify-ai
make pm-status
make pm-summary
make verify
git status --short
git log --oneline -1
```

## Required report file

`.pm/runtime/worker-report.md`

## If blocked

Write a blocker report to `.pm/runtime/blockers.md`. Do not invent product direction or change scope silently. Report exactly what is blocking and why.
