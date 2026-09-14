---
name: executing-plans
description: Carry out an approved written implementation plan in order, with critical review, task-level verification, and clear handling of drift or blockers. Use when a plan is the source of truth for a multi-step change; do not use to invent a missing design or plan while coding.
---

# Executing Implementation Plans

## Purpose

Implement an approved plan faithfully while using engineering judgment to detect mistakes, drift, and unsafe assumptions. Completion means the requested behavior is implemented and verified, not merely that the listed edits were attempted.

## Preflight Review

Read the entire plan and the referenced requirements or design. Inspect the current working state before changing it. In Git repositories, record the starting revision and identify pre-existing staged and unstaged changes. Confirm that the plan's assumptions, file paths, interfaces, dependencies, and validation commands still fit the repository.

Raise a question before starting if the plan has a critical gap, conflicts with current code, lacks required authority, or would make an irreversible or out-of-scope change. For a minor discrepancy with an obvious, behavior-preserving correction, record the adjustment and continue.

## Execute One Atomic Task at a Time

For each task:

1. Restate its intended deliverable and dependencies briefly.
2. Make only the changes needed for that deliverable, following the plan and local conventions.
3. Run the task's focused verification as soon as the change is ready.
4. Investigate and fix failures caused by the task; do not mark it complete while its required verification fails.
5. Review the diff for scope, interface consistency, and accidental changes. Check that implementation types preserve the agreed requiredness and invariants. For each added optional field, union variant, default, or fallback, identify the supported state or existing caller that requires it. Remove unsupported states and their branches and tests.
6. Once the deliverable and required verification are complete, immediately update the task's status in the implementation plan.
7. In Git repositories, create a local commit for the completed task before beginning the next task. Include its implementation, tests, documentation, and plan status update. Report the commit hash and a short description, then continue without waiting for approval.

Every task must have one status. Replace `Not started` with one concise factual update once the task is complete or blocked: `Complete — <result and verification>`, `Blocked — <specific reason>`, or `Skipped — <approved reason>`. Keep it to one line and at most 20 words; it provides execution context, not a narrative. Do not start the next task while the current task has an outdated or missing status.

Do not begin a dependent task until its prerequisite's completion condition is met. Commit at completed task boundaries, not on a timer; do not create empty, cosmetic, or incomplete checkpoints. Stage and commit only changes belonging to the task, preserving unrelated pre-existing staged and unstaged work. Do not push, amend, squash, or rewrite existing commits unless requested. If later verification exposes a defect in an earlier task, verify the correction and create a focused follow-up commit.

## Handle Deviations Explicitly

Plans are guides, not permission to force an invalid approach through the codebase. Widening a contract is a contract change, not an implementation detail; justify it with concrete evidence before revising the plan.

- **Small implementation detail differs:** choose the smallest change that preserves the plan's intent, document it, and verify it.
- **A task exposes a missing edge case or an inaccurate assumption:** update the plan with a concrete, atomic correction before continuing.
- **The goal, scope, public behavior, architecture, or risk profile must change:** pause and ask the user to approve the revised direction.
- **A blocker cannot be resolved from available evidence:** stop, report the blocker, what you tried, and the decision or resource needed.

Never hide a skipped verification, failed check, or unimplemented requirement behind a completion claim.

## Final Validation and Handoff

After all tasks are complete, run the plan's final validation and any proportionate repository checks. Review the cumulative changes from the recorded starting revision, together with any remaining uncommitted changes, against the goal and constraints, including compatibility, error behavior, documentation, and test coverage where applicable. Distinguish task changes from pre-existing work; an empty working-tree diff does not replace this review.

Report:

- What changed and the user-visible result.
- Verification performed and its outcome.
- Any intentional deviations from the plan.
- Remaining limitations, risks, or checks that could not be run.

If final validation exposes a defect, return to the relevant atomic task, repair it, and rerun the affected checks before declaring the work complete.
