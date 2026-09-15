---
name: harness-approve
description: 'Human Gate 1 — approve the spec/design before implementation begins'
persona: Gatekeeper
status: active

notes: |
  Human Gate 1. After /h:define produces the spec (spec.yaml, with design and tasks
  internalised), the human explicitly approves it before /h:build may run. In legacy
  standalone flows that branch design out, this same gate approves design.md.

gates:
  - check: 'spec.yaml exists (or design.md in legacy flows)'
    on_fail: STOP, run /h:define (or /h:design in legacy flows)
  - check: 'spec.yaml "Ref: PENDING" (or design.md "Ref: PENDING")'
    on_fail: STOP, already approved or rejected

actions:
  - present_spec_and_design_to_human
  - wait_for_explicit_human_approval
  - if_approved:
    - set_ref: 'APPROVED (spec.yaml; design.md in legacy flows)'
    - prompt_testing_level: confirm or select the feature Testing Level (S/M/L)
    - route: to /h:build
  - if_changes_requested:
    - set_ref: 'REJECTED'
    - route: to /h:define (or /h:design in legacy flows)

must_do:
  - Present the full spec (and design) to the human
  - Wait for explicit human approval
  - Confirm or select the Testing Level (S/M/L)
  - Update the Ref marker only after human approval

must_not_do:
  - Approve without human confirmation
  - Proceed to build without Ref: APPROVED
---
<!-- Reference contract. Runtime execution and enforcement are provided by the Supram engine (paid offering). -->
