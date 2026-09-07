---
name: writing-plans
description: Create an implementation plan for a confirmed multi-step change. Use when requirements or a design are available and the work should be decomposed into independently reviewable, testable tasks; do not use for a trivial single-change edit.
---

# Writing Implementation Plans

## Purpose

Produce a plan that another capable engineer can execute without rediscovering the intended architecture. It should state the target outcome, map the affected code, sequence the work, and make verification explicit.

Do not plan an unconfirmed or materially ambiguous design. Resolve those questions first. If the request contains several independent deliverables, propose separate plans or a deliberately scoped first slice.

Write the completed implementation plan to `docs/implementation plan/<slug>.md` in the repository that will be changed, unless the user specifies a different location. This written plan is the authoritative handoff; do not leave the plan only in chat.

## Gather Evidence Before Decomposing

Read the requirements or design and inspect the relevant repository areas. Identify existing conventions, entry points, tests, data models, public interfaces, configuration, build commands, and documentation that constrain the work. Record assumptions only where evidence is unavailable.

Stay consistent with the repository's organization. Do not introduce a broad restructuring solely to make the plan look cleaner.

## Make Tasks Atomic

An atomic task is a coherent vertical slice that can be implemented, reviewed, and verified on its own. It is not simply a short action.

Every task must:

- Deliver one observable capability, safeguard, or internal contract.
- Include all setup, implementation, tests, and documentation needed for that deliverable; do not strand scaffolding in its own task.
- Have a precise completion condition and a runnable verification method.
- Depend only on completed tasks or on interfaces that the plan defines explicitly.
- Be small enough for a reviewer to accept or reject without having to approve a neighboring task.

Split tasks when they create independently testable behavior or when one can be meaningfully accepted while another is rejected. Combine steps that only make sense together, such as a schema field, its validation, and its focused test. Order tasks so each adds a stable foundation for the next; avoid plans that require partially implemented future work to test an earlier task.

## Plan Format

Create the plan at the required location, then start it with:

```markdown
# [Change] Implementation Plan

**Goal:** [One sentence describing the outcome]

**Approach:** [Brief description of the selected design]

**Constraints:** [Only requirements that apply across tasks]

**Validation:** [The final test, build, lint, or manual checks]
```

Then write one section per atomic task:

```markdown
## Task 1 — [Deliverable]

**Status:** `Not started`

**Purpose:** [What becomes true after this task]

**Files:**
- Create: `path/to/new-file`
- Modify: `path/to/existing-file` — [specific responsibility]
- Test: `path/to/test-file`

**Dependencies and contract:**
- Requires: [earlier task or existing interface]
- Provides: [exact exported behavior, data shape, or UI contract]

**Steps:**
1. [Concrete edit or test action, including relevant names or behavior.]
2. [Next concrete action.]
3. Run: `[exact command]`.

**Done when:** [Observable behavior and expected verification result.]
```

Every task must have one status, initialized to `Not started`. During execution, the executor updates it when the task is complete or blocked. A status is a single short line—at most 20 words—not a progress essay. Use `Complete — <result and verification>`, `Blocked — <specific reason>`, or `Skipped — <approved reason>` as applicable.

Use real paths, names, commands, expected outcomes, and edge cases discovered during investigation. Include code snippets only when they prevent a consequential ambiguity; otherwise describe the desired behavior precisely. Never leave placeholders such as “add validation,” “handle errors,” or “write tests” without saying what must be validated, how failure behaves, and which check proves it.

## Review Before Handoff

Check the completed plan against the source requirements:

- Every requirement has a task or an explicit reason it is out of scope.
- Every task satisfies the atomic-task criteria and can be tested at the point it appears.
- Names, interfaces, paths, and task dependencies agree throughout.
- Commands are plausible for this repository, and validation covers both the new behavior and meaningful regression risk.
- No task relies on vague follow-up work or unspecified decisions.

Correct gaps in the plan before handing it off. State the plan location, the sequence of tasks, and any remaining assumptions or user decisions required before execution.
