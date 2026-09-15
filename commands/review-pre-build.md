---
name: harness-review-pre-build
description: >
  Sr Architect pre-flight review. Retired in v0.3.x. Kept for legacy compatibility only.
  Managers do not invoke this command for new slices.

persona: Sr Architect
status: retired
reason: Needs fresh perspective, separate from design Analyst
gates:
  - check: design.md exists
    on_fail: STOP, run /h:design first
  - check: spec.md exists
    on_fail: STOP, run /h:define first
  - check: BRD.md exists
    on_fail: STOP, no BRD to validate against

preflight:
  - read_deferred_ledger: if deferred.md exists in active feature, read it for reconciliation

actions:
  - derive_relevant_skills: read technology.yaml and map design scope to installed skills
  - load_relevant_skills: read only the installed skills relevant to the review scope
  - check_framework_capabilities: reject unjustified duplication of capabilities supplied by the selected stack
  - check_previous_review: if review-pre-build.md exists and has a FAIL verdict, extract its gap list. Focus exclusively on verifying these gaps are addressed in the design. Do NOT re-audit the entire design.
  - if no_previous_review_or_passed:
    - read: BRD.md (business requirements)
    - read: constitution.md (rules, constraints)
    - read: spec.md (feature requirements)
    - read: design.md (proposed architecture)
    - check: design covers ALL BRD requirements
    - check: design follows constitution constraints
    - check: design addresses all spec acceptance criteria
    - check: evidence contract is sufficient and proportionate before approval
    - check: no gaps between requirements and design
  - if gaps_found:
    - classify_each_finding: apply the blocker predicate (concrete evidence + invalidated approved contract + earliest corrective command). A finding is a blocker only if it satisfies ALL three predicate fields. Missing any field means the finding is deferred.
    - if blockers_found:
      - write: review-pre-build.md with FAIL + blocker gap list + route to /h:design
      - set: 'Ref: PENDING'
      - STOP: route back to /h:design
    - if only_deferred:
      - append_each_to_deferred: write each finding to deferred.md with ID, source, rationale, destination, status=open
      - write: review-pre-build.md with PASS (all findings deferred to ledger)
      - set: 'Ref: APPROVED'
      - route: to /h:approve (human gate)
  - if no_gaps:
    - write: review-pre-build.md with PASS
    - set: 'Ref: APPROVED'
    - route: to /h:approve (human gate)

outputs:
  - review-pre-build.md (PASS/FAIL + gap list)
  - deferred.md (if deferred findings were appended)

must_do:
  - Check for existing review-pre-build.md; if FAIL exists, focus on gap verification (only check what failed) rather than a full re-audit
  - Record consulted and missing skills in a Skill Evidence section
  - Benchmark design against the global Ponytail YAGNI policy (reject over-engineered architectures)
  - Reject UI bloat and over-engineered, bespoke front-end components, enforcing the Ponytail YAGNI Framework
  - Check every BRD requirement is in design
  - Check constitution constraints are followed
  - Check spec acceptance criteria are addressed
  - Report ALL gaps, not just critical ones
  - Challenge missing evidence and unnecessary evidence with equal rigor
  - Apply the blocker predicate to every finding: concrete evidence + invalidated approved contract + earliest corrective command
  - Append deferred findings to deferred.md with ID, source, rationale, destination, and status
  - MUST use the write tool to save the final report directly to disk. Do NOT output the report in your final chat response.

must_not_do:
  - Skip any BRD requirement
  - Ignore constitution constraints
  - Approve if gaps exist
  - Proceed to /h:approve without PASS
  - Defer evidence-scope decisions until pre-verify
  - Defer findings that satisfy the blocker predicate
  - Route deferred findings backward to design
---
<!-- Reference contract. Runtime execution and enforcement are provided by the Supram engine (paid offering). -->
