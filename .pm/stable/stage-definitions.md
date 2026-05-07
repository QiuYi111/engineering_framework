# Stage Definitions

## product_definition

Goal: Define what to build, for whom, and under what boundaries.

Allowed work:

- Ask product questions.
- Write `.pm/stable`.
- Initialize `.pm/runtime`.

Forbidden work:

- Implement product code.
- Start `/goal` loop before required files exist.

Exit criteria:

- Product, value proposition, journeys, roadmap, guardrails, and acceptance rubric exist.
- `product_definition_ready: true`.

## feasibility

Goal: Prove the long-running loop can execute safely in bounded mode.

Allowed work:

- Build loop runtime.
- Run `max_iterations: 5`.
- Test state updates, delegation, review, commits, stop conditions, and resume.

Forbidden work:

- Unbounded `/goal` without passing bounded validation.
- Auto-merge, push, deploy, or release.

Exit criteria:

- Five-iteration validation meets the 4/5 pass threshold.
- Failure breaker and stop conditions are verified.
- `feasibility_ready: true`.

## delivery

Goal: Use unbounded `/goal` dogfood to improve Harness itself.

Allowed work:

- Run long-lived supervisor loops.
- Delegate to intern.
- Commit per accepted task.
- Stop for user decisions.

Forbidden work:

- Product boundary changes without user approval.
- Core/infra/security/deployment work without explicit approval.
- Automatic merge, push, publish, or deploy.

Exit criteria:

- Stage goal is complete.
- Return summary and evidence are available.
- User approves next stage or stops.
