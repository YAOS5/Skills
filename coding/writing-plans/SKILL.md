---
name: writing-plans
description: Create an implementation plan for a confirmed multi-step change. Use when requirements or a design are available and the work should be decomposed into independently reviewable, testable tasks; do not use for a trivial single-change edit.
---

# Writing Implementation Plans

## Purpose

Produce a plan that another capable engineer can execute without rediscovering the intended architecture. It should state the target outcome, map the affected code, sequence the work, and make verification explicit.

Do not plan an unconfirmed or materially ambiguous design. Resolve those questions first. If the request contains several independent deliverables, propose separate plans or a deliberately scoped first slice.

Default to one combined design and implementation document at `docs/spec/<slug>.md` in the repository that will be changed. When a spec is supplied, refine it in place and append or update its `## Implementation plan` section; preserve a user-specified location. If the confirmed design exists only in chat, record it in the design sections before adding tasks. This combined document is the authoritative handoff; do not leave the plan only in chat.

Use a separate plan only when explicitly requested or when one specification supports multiple distinct implementation efforts. For that exception, use `docs/implementation plan/<slug>.md` unless directed otherwise, link to the authoritative spec sections, and keep shared constraints and contracts in the spec rather than copying them into the plan. Do not relocate or consolidate existing documents unless that is part of the requested work.

## Gather Evidence Before Decomposing

Read the requirements or design and inspect the relevant repository areas. Identify existing conventions, entry points, tests, data models, public interfaces, configuration, build commands, and documentation that constrain the work. Record assumptions only where evidence is unavailable.

Stay consistent with the repository's organization. Do not introduce a broad restructuring solely to make the plan look cleaner.

Choose one canonical representation and name for each shared concept. Reuse it across internal boundaries unless a different shape or trust boundary requires separation. Do not introduce compatibility aliases without an identified existing consumer; name that consumer in the plan.

## Agree Shared Contracts

After repository investigation, discuss any shared contracts that still require material choices in chat before writing the plan. Do not create or rewrite the implementation plan while those choices remain unresolved.

Discuss one contract at a time, starting with the most important or representative contract: the one that determines the core behavior or establishes a pattern for the others. Present a compact field list or table showing what information exists, why it is needed, who consumes it, and whether it is required or optional. Distinguish explicit requirements from inferred fields, highlight unresolved alternatives, and recommend the minimal sufficient shape. Do not present a batch of contracts for the user to review at once.

Model supported states only. For each optional field, state when absence is valid and what it means; uncertainty alone does not justify optionality. For each invariant, identify where it is established and which consumers may rely on it. Distinguish unvalidated external input from the contract used internally.

End the turn and wait for the user's response. Resolve the current contract before moving to the next, carrying agreed decisions forward. Reuse contracts already settled by the conversation or supplied specification without requesting approval again; discuss only remaining material choices. Leave routine naming, language syntax, and local implementation mechanics to engineering judgment.

Once the material contract choices are agreed, refine the document's authoritative contract definitions before writing tasks. Record canonical names, types, optionality, required invariants, error behavior, and side-effect ownership there once. Extend the design-level interfaces in place rather than adding another public-interface definition. Reference the named contracts from relevant tasks.

Synchronize agreed behavior changes and clarifications into the relevant design sections before handing off the plan; never defer this synchronization to an implementation task. Update affected contracts and task dependencies, consolidating overlapping prose without changing agreed requirements. The executor chooses implementation mechanics and surfaces necessary contract changes before introducing them.

## Agree End-to-End Verification Before Writing Tasks

Unit tests are required for implemented behavior; they do not replace verification of the integrated feature. Before creating or rewriting implementation tasks, discuss with the user how the feature will be verified beyond unit tests. Inspect existing integration tests, launchers, environment configuration, and supplied instructions first so the recommendation is concrete.

Recommend a representative path through the actual application and its real dependencies, including API endpoints where relevant. Explain what it will prove and what it cannot prove. A successful endpoint request alone does not establish that the application's end-to-end workflow works. For a feature with no external service, propose an appropriate integrated local or user-facing check instead.

Discuss one material verification decision at a time and wait for the user's response before proceeding with dependent planning. Start with the proposed scenario and environment, then resolve any missing access, permitted side effects, and success evidence. Reuse verification choices and authorization already supplied by the user; do not ask for them again. If the user explicitly asks to skip discussion, proceed with clearly stated assumptions and unresolved execution prerequisites rather than inventing access or authorization.

Agree the following where relevant, using repository evidence rather than asking the user to repeat known details:

- **Scenario and environment:** the normal entrypoint/user flow, real services and endpoints, sandbox/staging/production choice, test data, and any required timing or external state.
- **Access and instructions:** which credentials, account permissions, environment setup, and user-provided procedures the executor needs. Invite the user to supply missing setup instructions or provision credentials through the existing secret mechanism. Record credential locations, environment-variable names, or retrieval instructions in the plan, never secret values; do not request secrets be pasted into chat or committed files.
- **Permitted actions:** what reads and writes the check may perform, relevant limits or costs, and cleanup when needed. Record the scope of existing authorization so the executor need not ask again. Access to credentials alone does not authorize unrelated mutations, and planning must not perform external writes merely to design a check.
- **Pass/fail evidence:** the expected application outcome and independent service-side observation where available, including asynchronous completion, time bounds, and meaningful failure cases. Distinguish authentication/connectivity, request acceptance, and completed behavior; name which acceptance criteria each check actually proves.
- **Unavailable checks:** agree how missing credentials, environment availability, or an unobservable outcome will be handled. Identify the prerequisite and who supplies it. Fakes can supplement coverage but cannot silently substitute for the agreed real integration check.

Settle the verification approach before writing tasks; credentials themselves may be provisioned later under an explicit setup step. Do not leave a generic “ask the user how to test” or “run an integration check” for the executor. Record the agreed procedure once under `### Final verification`, with concrete commands or user actions, expected evidence, and completion conditions. Include access/setup references, permitted effects, cleanup, and unavailable-check procedures only when the chosen verification requires them; omit boilerplate about irrelevant services or credentials. Reference it from the tasks that enable it, and put earlier focused integration checks with their first runnable task.

Require the executor to record what was actually run, the observed result, and remaining gaps without exposing secrets. An unavailable required check remains blocked or unverified; passing unit tests does not make it complete. An explicitly agreed deferral must state the evidence gap and must not be described as end-to-end verification success.

## Plan Outcomes, Not Coding Instructions

Assume a capable implementer. Define each task's outcome, likely affected files, dependencies, verification, and completion condition. Leave routine coding steps and local implementation mechanics to the executor.

Prescribe a sequence only when correctness or safe operation depends on it, and explain why the order matters. Keep shared constraints and ordering invariants in the design or shared contracts and reference them from tasks. Do not turn removed task steps into an implementation recipe elsewhere in the document.

File lists are evidence-based starting points, not exhaustive edit mandates. Distinguish required interface locations from likely implementation files. Keep concrete verification procedures, including agreed access, side effects, cleanup, and evidence requirements.

## Review Implementation Complexity

Review the design and shared contracts, not just task wording. For each proposed abstraction, optional field, fallback, validation, or recovery path, identify the current requirement or consumer that needs it. Remove anything justified only by hypothetical callers or future use. Perform this review while planning; do not simply add a checklist for the executor in place of simplifying the design.

- Require dependencies needed by every supported execution path. Testability means supplying replacements, not adding missing-dependency fallbacks.
- Give each resource one clear owner.
- Validate external input at its boundary; internal consumers should rely on established contracts. Recheck state when it can change.
- Return structured data only when an identified consumer needs it.
- Catch exceptions only for a defined handling, boundary-translation, thread-transfer, or cleanup purpose.
- Model genuine absence and failure states explicitly; do not remove necessary safeguards merely to reduce branches.

Settle shared contract decisions that affect behavior or task dependencies. Leave local implementation mechanics to the executor when several approaches satisfy those contracts.

## Make Tasks Atomic

An atomic task is a coherent vertical slice that can be implemented, reviewed, and verified on its own. It is not simply a short action.

Every task must:

- Deliver one observable capability, safeguard, or internal contract.
- Include all setup, implementation, tests, and documentation needed for that deliverable; do not strand scaffolding in its own task.
- Have a precise completion condition and a runnable verification method.
- Depend only on completed tasks or on interfaces that the plan defines explicitly.
- Be small enough for a reviewer to accept or reject without having to approve a neighboring task.

Prefer the earliest runnable path through the intended workflow, using injected fakes where necessary. Being independently unit-testable is not sufficient reason for a separate task. Introduce supporting types with their first consumer; justify foundation-only tasks.

Split tasks when they deliver behavior that can be meaningfully accepted or rejected independently. Combine steps that only make sense together, such as a schema field, its validation, and its focused test. Avoid plans that require partially implemented future work to test an earlier task.

## Consolidate the Design Before Adding Tasks

Treat the document as the current implementation contract, not a record of the design conversation. Each planning pass may consolidate existing prose while preserving agreed requirements. Include only information needed to determine behavior, boundaries, interfaces, work sequence, or verification. Omit empty sections, settled-decision history, generic execution advice, and repeated summaries.

Before appending or rewriting tasks, compress the supplied design: merge overlapping sections, incorporate settled assumptions, and remove superseded wording and discussion history. Preserve agreed behavior, interfaces, invariants, scope, and material rationale. This is an editorial pass, not permission to change the design or reopen settled decisions. Keep existing task status and verification evidence that remains relevant; omit document-edit chronology.

Use conditional sections. Omit assumptions or unresolved-decisions sections when none remain; material design choices must be settled before task planning. Keep failure behavior beside the behavior it qualifies. Add a separate shared-contract section only when explicit interfaces or invariants warrant it. Reuse clear equivalent headings rather than renaming them to match a template.

## Plan Format

Keep the consolidated design ahead of implementation tasks. A compact combined document can use:

```markdown
# [Change]

## Outcome and scope
## Behavior and contracts
## Acceptance criteria

## Implementation plan
### Task 1 — [Observable outcome]
### Final verification
```

Each section must add information unavailable elsewhere in the document:

- Define outcome, scope, component ownership, and cross-task constraints in the design once. Do not repeat them in a plan preamble or task descriptions.
- Keep acceptance criteria as a short checklist of feature outcomes. Task checks cover focused verification; final verification covers integrated scenarios and regression. Do not repeat the entire edge-case inventory at each level.
- Reference named contracts and acceptance criteria where useful. Explicitly checking a critical safeguard is valuable coverage; restating its full policy is not.
- Omit execution guidance by default. Add it only for project-specific execution instructions not already available in the design or repository guidance.

Write one subsection per atomic task:

```markdown
### Task 1 — [Observable outcome]

**Status:** `Not started`
**Depends on:** [Earlier task or interface, only when non-obvious]

**Change:** [What becomes true; reference relevant contracts.]
**Files:** [Likely implementation and test paths, with responsibilities where useful.]
**Verify:** [Specific focused checks, expected results, and runnable command.]
```

Delivering the stated outcome and passing its checks is the completion condition. Add a separate condition only when that is insufficient. Do not add Purpose, Provides, or Done when fields that restate the same outcome. Task descriptions add implementation scope, necessary dependencies, and runnable checks; they must not retell the design. Mark a file location as required only when an existing interface or explicit requirement fixes it.

Put the agreed integrated verification procedure and final regression, build, lint, or manual checks under `### Final verification`. Keep focused checks with their first runnable task and reference them rather than duplicating them in final verification. Record applicable setup, effects, cleanup, evidence, and limitations once where the check is defined.

For an intentionally separate plan, use an implementation-plan title and source-spec links, then the same task and final-verification structure at appropriate heading levels.

Every task must have one status, initialized to `Not started`. During execution, the executor updates it when the task is complete or blocked. A status is a single short line—at most 20 words—not a progress essay. Use `Complete — <result and verification>`, `Blocked — <specific reason>`, or `Skipped — <approved reason>` as applicable. Record actual verification results and remaining gaps concisely, without repeating the procedure or adding edit history.

Use real paths, names, commands, expected outcomes, and meaningful edge cases discovered during investigation. Include code snippets only when they prevent a consequential ambiguity. Never leave placeholders such as “add validation,” “handle errors,” or “write tests” without specifying the relevant behavior and how it is verified.

## Review Before Handoff

Check the completed plan against the source requirements. Ask: **What complexity does this plan introduce, and which requirement makes each part necessary?**

- Every requirement has a task or an explicit reason it is out of scope.
- Every planned field, abstraction, and behavior has a requirement or concrete implementation need. Resolve material shared-contract alternatives to one sufficient choice; leave local implementation mechanics to the executor. Remove additions justified only by possible future use.
- Every task satisfies the atomic-task criteria and can be tested at the point it appears. It specifies an outcome and verification rather than routine coding steps; any prescribed sequence has a concrete correctness or safety reason.
- Names, interfaces, paths, and task dependencies agree throughout. Each shared requirement and contract has one authoritative definition, and all agreed clarifications are already reflected in the design.
- Commands are plausible for this repository, and validation covers both the new behavior and meaningful regression risk.
- Verification beyond unit tests was discussed or already settled by the user (or discussion was explicitly declined). The plan identifies the actual integrated scenario and pass/fail evidence, plus access/setup instructions, permitted actions, and handling of unavailable checks where applicable; it does not defer designing verification to execution.
- No task relies on vague follow-up work or unspecified decisions.

Finish with a deletion pass over the entire combined document. For each paragraph, ask: **What implementation decision or verification action would become ambiguous if this disappeared?** Delete or merge paragraphs with no answer. Preserve critical safeguards and necessary detail; do not impose a hard word limit.

Correct gaps in the plan before handing it off. State the combined document location (or explicit separate-plan location), the sequence of tasks, and any remaining execution assumptions or prerequisites. Do not hand off material design decisions as implementation assumptions.
