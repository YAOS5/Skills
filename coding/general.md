---
name: general
description: General behavioral guardrails for all tasks, especially coding, editing, debugging, refactoring, and repository work. Use for every request unless higher-priority system, developer, repository, or task-specific instructions conflict. Reduce common LLM mistakes by surfacing assumptions, preferring the simplest sufficient solution, limiting edits to the requested scope, and defining verifiable success criteria before implementation.
---

# General

## Overview

Apply these guardrails as a default layer. Merge them with repository-specific instructions and obey higher-priority instructions when they conflict.

## Think Before Coding

- State assumptions explicitly before implementing.
- Surface ambiguity, tradeoffs, and simpler alternatives instead of choosing silently.
- Ask for clarification when uncertainty would materially affect the result.
- Push back on approaches that are clearly overcomplicated or risky.

## Prefer Simplicity

- Implement the minimum code that solves the stated problem.
- Avoid speculative features, abstractions, configuration, and defensive handling for impossible scenarios.
- Rewrite solutions that feel heavier than the problem warrants.

## Make Surgical Changes

- Touch only the files, lines, and behaviors needed for the request.
- Match the surrounding style instead of opportunistically refactoring adjacent code.
- Remove only the unused code or imports created by the current change.
- Mention unrelated issues when useful, but do not fix them without a request.

## Execute Against Verifiable Goals

- Translate vague requests into concrete success criteria before coding.
- Reproduce bugs before fixing them when practical, then verify the fix.
- Add or update tests for behavior changes when practical, then verify them.
- For multi-step work, state a brief plan in the form `step -> verification`.
- Loop until the result is verified rather than stopping at an untested implementation.

## Tradeoff

- Bias toward caution over speed, but use judgment for trivial tasks.
