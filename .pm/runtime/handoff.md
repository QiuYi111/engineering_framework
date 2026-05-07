# Handoff

## Current State

Product contract is frozen. Harness is in feasibility stage for dogfood-grade `/goal` long-running supervisor loop.

## Read First On Resume

1. `.pm/runtime/state.yaml`
2. `.pm/runtime/active-stage.md`
3. `.pm/stable/roadmap.md`
4. `.pm/stable/product.md`
5. `.pm/runtime/loop-log.md`
6. `.pm/runtime/loop-control`

## Next Expected Action

Create a bounded `next-task.md` for implementing the first executable supervisor loop runtime capability. The task should preserve user changes and respect the existing modified `scripts/harness_runtime/verify.py`.

## Important Constraints

- Do not overwrite user changes.
- Supervisor manages branch policy.
- Intern must create a clear commit for each accepted task.
- No auto-merge, push, publish, or deploy.
- Stop for core, infra, security, auth, payment, deployment, or product-boundary changes.

## Current User Decision

The user approved dogfood MVP, no visual UI, `/goal` long-run requirement, sync worker mode for first validation, and branch/commit separation.
