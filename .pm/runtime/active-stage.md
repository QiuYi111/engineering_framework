# Active Stage

## Stage

delivery

## Goal

Use unbounded `/goal` dogfood to improve Harness itself after bounded feasibility passed.

## Current Objective

Harden the supervisor loop based on dogfood findings: explicit slash-command delegation, independent review agents, auditable commit taxonomy, and Makefile-based verification.

## Exit Criteria

- Harness improves itself through at least one meaningful dogfood goal after Stage 1.
- The user can audit the run from git history and `.pm/runtime`.
- Independent review evidence exists for material worker claims.
- Stop conditions work without manual babysitting.

## Next Expected Action

Supervisor should delegate the slash-command protocol hardening task to OpenCode Intern.
