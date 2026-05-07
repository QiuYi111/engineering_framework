# Evidence

## Status

Evidence status: waived_by_user for external demand validation.

This is a Builder-mode personal dogfood tool. The first proof is not market demand; it is whether Harness can improve Harness itself through long-running autonomous PM loops without losing control or maintainability.

## Existing Evidence

- The repository already contains product discovery, supervisor, intern, PM runtime templates, and deterministic CLI verification.
- Current `uv run harness verify-ai --project /Users/qiujingyi.7/Harness` passes with 47 checks, 0 failures, and 1 warning.
- Current implementation already supports the protocol layer for PM files, worker reports, safety flags, and loop control.
- The gap identified during discovery is executable long-run reliability, not lack of product concept.

## Dogfood Evidence To Collect

- Five consecutive supervisor iterations on Harness itself.
- Valid task, report, review, loop-log, state, and handoff artifacts for each iteration.
- Per-iteration git commits created by intern and reviewed by supervisor.
- Fresh verification output attached to each accepted report.
- Correct stop behavior when a stop condition is deliberately triggered.

## Waiver

External user demand validation is intentionally waived for v0 because the product is first built as a personal dogfood tool. Revisit demand validation only after dogfood evidence proves the loop is reliable.
