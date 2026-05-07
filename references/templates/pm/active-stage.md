# Active Stage

## Stage ID

product_definition

## Stage goal

Define what to build, for whom, and why it matters.

## Why this stage matters

Without a clear product contract, engineering effort is wasted on the wrong things.

## Inputs

- User's raw product idea
- Any existing product notes

## Allowed work

- Product discovery
- UX research
- UI direction exploration
- Evidence validation

## Forbidden work

- Code implementation
- Architecture decisions beyond guardrails

## Exit criteria

- [ ] EC-001: product.md complete
  Evidence:
  Blocking: true
- [ ] EC-002: evidence.md passed or waived
  Evidence:
  Blocking: true
- [ ] EC-003: ux-principles.md complete
  Evidence:
  Blocking: true
- [ ] EC-004: user-journeys.md complete
  Evidence:
  Blocking: true
- [ ] EC-005: ui-direction.md complete if required
  Evidence:
  Blocking: false
- [ ] EC-006: All readiness flags true where applicable
  Evidence:
  Blocking: true

## Stage exit evaluation

After each accepted task, the Supervisor MUST evaluate current stage exit criteria:
1. Check each EC item above. If `Evidence` is filled and `Blocking: true`, mark as satisfied.
2. If ALL blocking exit criteria are satisfied: write `STAGE_EXIT_REACHED` to loop-control.
3. If any blocking exit criteria remain unsatisfied: write `CONTINUE` to loop-control.
4. Never mark a stage as complete without evidence for each blocking criterion.

## Current progress

Not started.

## Open blockers

None.
