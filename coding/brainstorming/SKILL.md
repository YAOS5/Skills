---
name: brainstorming
description: Shape an ambiguous product, feature, or behavior-change request into an agreed design before substantial implementation. Use when intent, scope, constraints, or the approach needs discovery; do not use for straightforward, fully specified edits.
---

# Brainstorming

## Purpose

Turn an initial idea into a decision-ready design and record it as a durable specification. For work that may be implemented, the required outcome is a self-contained spec at `docs/spec/<slug>.md` in the repository the work will change. This document will also hold the implementation plan when planning begins. Do not leave an agreed design only in chat.

An explicitly answer-only feasibility investigation may end with a recommendation instead of a spec. Any request that proceeds toward a change must produce the spec before implementation planning begins.

## Choose the Right Depth

Classify the request before diving into details and say which path you are taking.

- **Exploration:** Answer a feasibility question or test an assumption. State the question, the smallest safe investigation, and what evidence would answer it. Treat temporary experiments as disposable unless the user later asks to keep them.
- **Focused change:** An existing, well-understood flow needs a small alteration. Inspect the relevant code or materials, ask only the questions that affect the result, then present a concise proposed change, affected areas, and validation. Get confirmation when the request leaves a meaningful design choice open.
- **Design work:** A new capability, a cross-cutting change, or a request with important unknowns. Explore the context, clarify the goal and constraints, compare viable approaches, then present a structured design for approval before committing to implementation.

If a focused change reveals new dependencies, interface changes, or several independently useful deliverables, stop and reclassify it as design work. Split an oversized request into the smallest valuable first slice before designing it.

## Understand the Problem

Start with the available project context: relevant files, documentation, current behavior, conventions, and recent changes when they matter. Then determine:

- The user outcome and how success will be observed.
- Boundaries: what is explicitly in and out of scope.
- Constraints such as compatibility, performance, data handling, schedule, or design conventions. This is mandatory and highly important.
- Existing behavior and integrations that must remain intact.

Do not make the user repeat facts the project already supplies. Ask only questions whose answer can materially change the scope, behavior, approach, risk treatment, or success criteria.

## Interview the Decision, Not the User

Proactively uncover consequential behavioral choices through an interactive interview. Do not wait for the user to request a blind-spot pass or for an ambiguity to announce itself. This is part of brainstorming, not a separate workflow; never pursue a predetermined number of questions.

Before asking, investigate what the repository, supplied references, and stated constraints can answer. Treat an existing implementation, design, or library named by the user as the default behavioral reference, while examining how the proposed change interacts with it. Never ask a broad “what do you mean?” question when the project context can narrow it first.

Before settling the design, walk through a concrete scenario from trigger to outcome and actively look for choices hidden in the proposed flow:

- What runs first, what information is available at each decision, and whether a different order changes the outcome.
- What gates, skips, or ends work; whether taking one action prevents another; and what happens when multiple actions are eligible.
- What happens when a step fails, returns no result, or only partly succeeds: whether later work continues, retries, or stops.

Use these prompts to discover relevant decisions, not as a questionnaire to recite. For each consequential choice the user has not settled, explain the concrete consequence and ask one question before adopting it, even when one option seems natural. For example: “With one decision call, putting the close decision before analysis means this iteration cannot consider entry. Should taking a close action end the iteration?” Keep routine implementation mechanics with the agent.

After each answer, trace its consequences through the scenario again: an answer about call count, ordering, or failure handling may expose another decision. Resolve those consequences before synthesizing the design; a general request to simplify a flow does not settle which work may be skipped.

Ask questions in the order that unlocks the next decision:

1. Confirm the outcome if the intended user or business result is unclear.
2. Define the boundary when it is unclear what belongs in this change.
3. Identify the existing pattern or reference to extend, if one may exist.
4. Choose an approach when more than one credible option remains.
5. Expose the highest-risk failure, constraint, or irreversible consequence.
6. Define how the result will be proved to work.
7. Look for a concrete way to reduce scope or complexity before synthesizing the design.

Skip questions already settled by the user's instructions or agreed behavior. Repository evidence should narrow questions, but the fact that a flow is implementable does not settle whether its behavioral consequences are wanted. End the interview when the scenario walkthrough exposes no remaining consequential choices that require guessing; do not continue merely to make the process feel thorough.

**Ask exactly one question per turn.** That question must address the single most important unresolved decision. After asking it, stop and wait for the user's answer; do not combine it with a follow-up question, a questionnaire, or a list of decisions for the user to answer at once. Once the answer resolves that decision, incorporate it into the working understanding, identify the next highest-value unresolved decision, and ask that next question in a new turn. If the answer does not resolve the current decision, clarify the same decision before moving on.

Every interview question must include all of the following:

- The exact decision being made and why it affects the design.
- A concrete recommendation grounded in observed code, a supplied reference, or an explicit constraint.
- The relevant alternative or trade-off when there is one.
- Specific names—such as a module, user flow, interface, or behavior—rather than generic labels.

If a recommendation cannot yet be specific, investigate further before asking. Make vague language testable by proposing a sharper interpretation and asking the user to confirm or correct it. For example, turn “make it faster” into a proposed operation and measurable target; turn “improve the UX” into a named user flow and the friction to remove; turn “add error handling” into the failure case and expected user-visible behavior.

When asked for a blind-spot pass, first summarize the relevant project conventions or prior decisions and the likely technical or operational pitfalls that the user may not know to ask about. Keep it concise and evidence-based, then continue with only the decisions that remain unresolved.

If the user asks to skip questions or to proceed, respect that instruction. Draft from the available evidence, label material assumptions, and identify any unresolved choice at the point where it affects the design. Do not turn a declined interview into an approval barrier.

## Explore Options Deliberately

For design work, offer two or three materially different approaches when there is a real choice. Explain the trade-offs in terms of user value, complexity, risk, and ongoing maintenance. Lead with a recommendation and keep unnecessary features out of every option.

When the direction is clear, present a design scaled to the work. Cover the parts that could otherwise surprise an implementer: component boundaries, data or control flow, interfaces, failure behavior, migration or compatibility implications, and validation. Once the design is settled, write it to `docs/spec/<slug>.md` in the relevant repository. The spec must state the outcome and acceptance criteria, selected approach and rationale, scope boundaries, affected areas or interfaces, constraints and failure behavior, validation, and any material assumptions.

Before substantial implementation, confirm that the proposed outcome and scope are acceptable whenever they were not already specified by the user. Incorporate requested changes and reconfirm the changed portion.

## Design Quality Checks

- Give each component one clear responsibility and a defined interface.
- Follow established project patterns unless changing a pattern is necessary to meet the goal.
- Include only adjacent cleanup that directly reduces the risk or cost of the requested work.
- Prefer a design that can be tested at its boundaries and understood without reading every implementation detail.
- Resolve ambiguity in writing rather than leaving important decisions for an implementer to guess.

## Document Structure and Ownership

Require the necessary information, not a fixed set of headings. Use clear headings scaled to the work; a compact design can use:

```markdown
# [Change]

## Outcome and scope
## Behavior and contracts
## Acceptance criteria
```

Separate approach, constraints, or shared contracts only when doing so makes substantial content easier to retrieve. Put failure behavior beside the behavior it qualifies. Include assumptions or unresolved decisions only when material ones remain; incorporate settled decisions into their authoritative sections.

Treat the document as the current implementation contract, not a record of the design conversation. Each planning pass may consolidate existing prose while preserving agreed requirements. Include only information needed to determine behavior, boundaries, interfaces, work sequence, or verification. Omit empty sections, settled-decision history, generic execution advice, and repeated summaries.

Give each requirement and shared contract one authoritative definition. Describe component responsibilities and ownership in the design; leave file/edit inventories to implementation tasks. Keep acceptance criteria as a short checklist of observable outcomes, referencing detailed behavior instead of repeating its edge cases. Record the validation approach briefly; planning adds the concrete checks that prove it.

Brainstorming establishes interface responsibilities, observable behavior, and material invariants. Specify exact types or signatures when they affect a design decision; otherwise leave that precision for planning to add in place. Do not create a second public-interface definition elsewhere in the document.

The writing-plans stage consolidates these sections before appending or updating `## Implementation plan`. Agreed clarifications replace or refine their authoritative definitions rather than accumulating addenda. Do not generate implementation tasks during brainstorming or add an empty plan as a placeholder. When revising a document that already contains tasks, preserve their deliverables and status and identify affected dependencies; their wording may be consolidated without losing requirements.

Before handoff, review every paragraph: **What implementation decision or verification action would become ambiguous if this disappeared?** Delete or merge paragraphs with no answer. Preserve critical safeguards explicitly, defining their behavior once. Do not use a hard word limit to remove necessary detail.

## Handoff

For every implementation-bound request, finish by writing the approved, self-contained design spec to `docs/spec/<slug>.md` in the repository that contains the affected work. Report that path and hand off the same document for an atomic implementation plan to be appended. Default to one combined document; use separate plans only when explicitly requested or when one specification supports multiple distinct implementation efforts. In that case, keep shared design and contracts authoritative in the spec and link to them from each plan. A feasibility finding may end with a recommendation only when the user explicitly asked for investigation rather than a change.
