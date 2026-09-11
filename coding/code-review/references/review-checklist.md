# Review Checklist

Use this checklist while reviewing. Report only meaningful findings.

## 1) Unused Includes / Imports

Check for includes/imports/usings that are no longer referenced after refactors.

Common signals:
- File compiles or runs with import removed.
- Linter flags: unused import, unused using, redundant include.
- Wildcard imports hide unused symbols.

Actions:
- Remove unused imports/includes.
- Replace wildcard imports with explicit imports when practical.

## 2) Dead Code

Check for code that cannot execute or has no effect.

Common signals:
- Unreachable branches after early return/raise/break.
- Feature-flag branches that are permanently disabled.
- Functions, methods, or constants no longer referenced.
- Assignments whose values are never read.

Actions:
- Delete dead branches and orphaned symbols.
- Confirm no dynamic/reflection-based usage before removal.

## 3) Simplification Opportunities

Check whether behavior is correct but implementation is harder than necessary.

Common signals:
- Deep nesting where guard clauses would be clearer.
- Repeated logic that can be extracted.
- Manual loops that can be replaced by standard library helpers.
- Overly abstract patterns for simple one-path behavior.

Actions:
- Prefer straightforward control flow.
- Reduce temporary state and duplicated branches.
- Keep abstractions only when they remove meaningful repetition.

## 4) Unit-Test Gaps

Check whether tests cover changed behavior and failure paths.

Minimum expectations:
- New behavior has unit tests.
- Bug fixes include regression tests.
- Error paths and boundary cases are asserted.
- Tests verify outcomes, not only that code executes.

Common missing cases:
- Null/empty inputs
- Off-by-one boundaries
- Invalid argument handling
- Concurrency or ordering assumptions

## 5) Style Inconsistencies

Check consistency with local repository conventions first, then language defaults.

Check for:
- Naming mismatches in the same module.
- Inconsistent formatting or import ordering.
- Mixed error-handling styles without rationale.
- Inconsistent docstring/comment style.

Actions:
- Align with existing lint/formatter rules.
- Flag deviations only when they hurt readability or maintenance.

## Reporting Rules

- Prioritize correctness and regression risk over cosmetic issues.
- Avoid speculative findings; tie each issue to concrete evidence.
- Include file references for every reported issue.
- If uncertain, mark as a question and state what verification is needed.
