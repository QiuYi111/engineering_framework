# UX Principles

## No New App

Harness lives inside the user's preferred coding agent. It should not introduce a separate UI for the dogfood MVP.

## Visible Autonomy

The user should see enough of the PM loop to trust that Harness is governing work rather than free-form chatting.

## Reversibility

Every meaningful worker iteration should be recoverable through git. Supervisor owns branch policy. Intern owns per-task commits.

## Truthful Reports

Worker claims are not accepted without evidence. Supervisor reviews report structure, verification output, git diff, commit hash, and scope.

## State On Disk

Critical state must live in `.pm/runtime`, not only in chat context.

## Stop Rather Than Drift

Harness should stop on unclear product direction, MVP boundary change, high-risk work, missing evidence, repeated failure, dirty git ambiguity, or stage exit.

## Resume From Artifacts

After interruption, Harness resumes from `handoff.md`, `state.yaml`, `loop-log.md`, and `loop-control`.

## Text Hygiene

Text output should be concise enough to scan, but this is basic hygiene rather than a differentiating product requirement.
