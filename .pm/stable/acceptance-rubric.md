# Acceptance Rubric

## Task Acceptance

A worker task is accepted only if:

- The objective matches `next-task.md`.
- Changed files stay within allowed scope.
- Forbidden scope was not touched.
- Verification commands were run or explicitly justified.
- Test results are fresh and attached.
- Git status and commit hash are reported.
- Acceptance criteria are checked.
- Deviations are disclosed.

## Iteration Acceptance

A supervisor iteration is valid only if:

- `next-task.md` exists and is bounded.
- `worker-report.md` exists and contains required sections.
- `acceptance-review.md` records accepted, rejected, or blocked with evidence.
- `state.yaml` is updated.
- `loop-log.md` has an append-only entry.
- `handoff.md` states the next expected action.
- `loop-control` is correct.

## Long-Run Validation

Bounded validation passes only if:

- `max_iterations: 5` was used.
- At least 4 of 5 iterations are valid.
- Accepted iterations have commits.
- Stop conditions are tested or explicitly exercised.
- Resume behavior is verified.

## Rejection Conditions

Reject or stop if:

- Report lacks evidence.
- Tests fail without a clear recovery plan.
- Git state is ambiguous.
- Scope is exceeded.
- Risk escalates to core or infra.
- Product boundary changes are needed.
- The same class of failure repeats too many times.
