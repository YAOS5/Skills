---
name: general
description: General behavioral guardrails for all tasks, especially coding, editing, debugging, refactoring, and repository work. Use alongside relevant task-specific skills unless higher-priority system, developer, repository, or task-specific instructions conflict. Reduce common LLM mistakes by surfacing material assumptions, preferring the simplest sufficient solution, limiting edits to the requested scope, and defining verifiable success criteria before implementation.
---

# General

## Overview

Apply these guardrails as a default layer alongside relevant task-specific skills. Task-specific skills define the workflow; these guardrails remain in force unless they conflict with higher-priority or more specific instructions. Merge them with repository-specific instructions and obey higher-priority instructions when they conflict.

## Think Before Coding

- Identify material assumptions and unresolved decisions before implementing.
- Ask for clarification when uncertainty would materially affect correctness, scope, or an irreversible action.
- Surface a simpler or safer alternative when it would materially improve the result.

## Prefer Simplicity

- Implement the minimum code that solves the stated problem.
- Avoid speculative features, abstractions, configuration, and defensive handling for impossible scenarios.

## Make Surgical Changes

- Touch only the files, lines, and behaviors needed for the request.
- Match the surrounding style instead of opportunistically refactoring adjacent code.
- Remove only the unused code or imports created by the current change.
- Mention unrelated issues when useful, but do not fix them without a request.

## Execute Against Verifiable Goals

- Translate vague requests into concrete success criteria before coding.
- Reproduce bugs before fixing them when practical, then verify the fix.
- Add or update tests for behavior changes when practical, then verify them.
- Loop until the result is verified rather than stopping at an untested implementation.
