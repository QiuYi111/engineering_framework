# Architecture Guardrails

## Current Product Form

Harness is a skill pack plus thin CLI runtime. Product behavior is expressed through skills, deterministic CLI gates, templates, and `.pm` runtime files.

## First Runtime Direction

The long-run loop should be implemented as a small deterministic runtime layer where possible, with judgment still handled by the supervisor skill.

## Branch And Commit Policy

- Supervisor owns branch creation and branch policy.
- Intern owns one clear commit per accepted task.
- Supervisor must verify git status, diff scope, commit hash, and evidence before continuing.
- No automatic merge to the base branch.
- No automatic push unless explicitly approved.

## Allowed Changes For Dogfood MVP

- CLI commands for PM runtime operation.
- `.pm` template/schema improvements.
- Supervisor/intern protocol refinements.
- Tests for runtime state, report validation, safety gates, and resume.
- Documentation needed to make `/goal` usable.

## Forbidden Without User Approval

- Changing core product positioning.
- Expanding MVP boundary.
- Changing the primary agent architecture.
- Security, auth, payment, deployment, or data migration behavior.
- Auto-merge, auto-push, auto-release, or auto-deploy.

## Risk Areas

- Trusting worker reports without independent verification.
- Losing work due to poor git boundaries.
- Infinite loops that keep creating low-value tasks.
- State drift between chat context, `.pm/runtime`, and git.
- Supervisor silently changing product direction.

## Verification Requirements

- Unit tests for deterministic runtime behavior.
- Integration-style dogfood test for five loop iterations.
- Explicit evidence in worker reports.
- Supervisor review before state advances.
