---
name: code-review
description: Perform focused code reviews of pull requests, patches, and local diffs. Use when Codex is asked to review code quality or identify risks, especially for unused includes/imports, dead code, opportunities to simplify implementations, unit-test coverage gaps, and code style inconsistencies.
---

# Code Review

## Overview

Review code with a risk-first mindset. Prioritize correctness and maintainability findings over summaries.

## Workflow

1. Read the full diff and determine the intended behavior change.
2. Identify modified execution paths, data flow, and externally visible behavior.
3. Run available checks (tests, linters, type checks) when feasible.
4. Apply the focused checklist in `references/review-checklist.md`.
5. Report findings in severity order with precise file references.

## Output Contract

Present findings first.

For each finding, include:
- Severity (`high`, `medium`, `low`)
- What is wrong
- Why it matters (bug risk, maintainability cost, or behavior regression)
- Evidence with file reference and line
- Recommended fix

After findings, include:
- Open questions or assumptions
- Brief change summary only if useful
- Residual test risk if coverage is incomplete

If no issues are found, explicitly state that no findings were detected and call out remaining test/validation risk.

## Review Heuristics

Check the following areas in every review:
- Unused includes/imports/usings
- Dead or unreachable code
- Opportunities to simplify implementations
- Unit-testing gaps and missing edge-case assertions
- Style inconsistencies with repository conventions

Load `references/review-checklist.md` for concrete checks and language-specific hints.
