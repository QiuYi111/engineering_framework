# Active Stage

## Stage

feasibility

## Goal

Prove that Harness can execute a dogfood-grade `/goal` supervisor loop on Harness itself.

## Current Objective

Build and validate the minimum executable runtime needed for five supervised iterations with sync OpenCode delegation, git commit checkpoints, report review, state updates, stop conditions, and resume.

## Exit Criteria

- `max_iterations: 5` bounded validation can run.
- At least 4 of 5 iterations produce valid task/report/review/state/log/handoff artifacts.
- Accepted iterations have intern-created commits.
- Supervisor verifies git status, commit hash, scope, and test evidence.
- Resume from runtime artifacts is tested.
- Stop conditions work.

## Next Expected Action

Supervisor should create the first bounded implementation task for the executable loop runtime.
