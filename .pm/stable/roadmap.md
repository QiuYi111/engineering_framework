# Roadmap

## Stage 0: Product Contract Freeze

Goal: Freeze product definition and runtime expectations for dogfood.

Exit criteria:

- `.pm/stable` files complete.
- `.pm/runtime` initialized.
- UI marked not applicable.
- MVP boundary agreed.

## Stage 1: Long-Run Feasibility Runtime

Goal: Build the minimum executable supervisor loop runtime that can run a bounded five-iteration dogfood test.

Included:

- Start/resume loop command or equivalent executable entrypoint.
- Read/write `.pm/runtime` state.
- Generate bounded task packets.
- Execute sync OpenCode delegation.
- Validate worker report structure.
- Check git status, commit hash, and allowed scope.
- Update acceptance review, loop log, handoff, state, and loop-control.
- Enforce stop conditions and failure breaker.

Exit criteria:

- A five-iteration run on Harness itself completes or stops for a correct reason.
- At least 4 of 5 iterations are valid.
- Each accepted iteration has a commit and verification evidence.
- Resume path is tested.

## Stage 2: Unbounded `/goal` Dogfood

Goal: Enable long-running goal mode after Stage 1 validation passes.

Included:

- Allow `max_iterations: null` after safety validation.
- Continue until stop condition, stage exit, or user interruption.
- Generate return summaries.
- Strengthen independent verification.

Exit criteria:

- Harness improves itself through a meaningful dogfood goal.
- The user can audit the full run from git and `.pm/runtime`.
- Stop conditions work without manual babysitting.

## Stage 3: Hardening

Goal: Make the loop reliable enough for repeated personal production use.

Included:

- Better worker timeout handling.
- More robust report schema validation.
- Optional reviewer role.
- Branch checkpoint policy.
- CLI status for PM runtime.

Exit criteria:

- Multiple dogfood projects complete without state drift.
- Failure modes are documented and recoverable.
