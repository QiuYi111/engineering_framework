# Handoff

## Current State

Product contract is frozen. Harness is in delivery stage for unbounded `/goal` dogfood on Harness itself.

## Read First On Resume

1. `.pm/runtime/state.yaml`
2. `.pm/runtime/active-stage.md`
3. `.pm/stable/roadmap.md`
4. `.pm/stable/product.md`
5. `.pm/runtime/loop-log.md`
6. `.pm/runtime/loop-control`
7. `.pm/runtime/acceptance-review.md`

## Next Expected Action

Continue Stage 2 dogfood by selecting the next bounded Harness improvement task and delegating it through `/harness-intern`.

## Important Constraints

- Do not overwrite user changes.
- Supervisor manages branch policy.
- Intern creates one clear product commit per accepted task or rework.
- Supervisor records PM ledger commits separately.
- Existing modified files below predate this task and are forbidden scope unless the user explicitly asks to work on them:
  - `scripts/harness_runtime/verify.py`
  - `subskills/opencode-cli/SKILL.md`
  - `subskills/opencode-cli/references/patterns.md`
- No auto-merge, push, publish, or deploy.
- Stop for core, infra, security, auth, payment, deployment, or product-boundary changes.

## Accepted Iterations

- Iteration 1: `pm-status` runtime health check; accepted after rework (`6125f40`, `07d4af8`).
- Iteration 2: strict worker-report validation; accepted (`8cfc1e6`).
- Iteration 3: deterministic `pm-next`; accepted with branch-policy warning remediated (`9265b88`).
- Iteration 4: branch-policy validation; accepted (`df9f58e`).
- Iteration 5: deterministic `pm-resume`; accepted (`b8aab4e`).
- Iteration 6: slash-command delegation, independent review guidance, commit taxonomy, and Makefile entrypoints; accepted after rework (`88302a4`, `a5d15b5`).

## Current User Decision

The user approved dogfood MVP, no visual UI, `/goal` long-run requirement, sync worker mode for first validation, branch/commit separation, independent reviewer agents, Makefile convention, and continued Stage 2 dogfood.
