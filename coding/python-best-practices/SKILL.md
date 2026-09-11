---
name: python-best-practices
description: Use when writing, reviewing, or refactoring Python code in this repository, especially for module structure, typing, error handling, logging, and tests.
---

# Python Best Practices

## Overview

Use this skill for any `*.py` change in this repository. Focus on readability, correctness, and maintainability while keeping code idiomatic and dependency-light.

## Guidelines

- Write clear, Pythonic code with simple control flow, small functions, and explicit names.
- Use type hints on public functions and complex data structures; keep hints accurate and minimal.
- Prefer `pathlib` over `os.path` for filesystem paths.
- Use context managers (`with`) for files, locks, and resources.
- Avoid mutable default arguments; use `None` and initialize inside the function.
- Do not shadow built-ins (`list`, `dict`, `id`, etc.).
- Prefer f-strings for formatting.
- Handle errors intentionally: catch specific exceptions and re-raise with useful context when needed.
- Use `logging` instead of `print` for non-trivial output.
- Keep modules import-safe: avoid side effects at import time and use `if __name__ == "__main__":` for scripts.
- Add concise docstrings for public modules, classes, and functions.
- Keep dependencies minimal and use the standard library when reasonable.
- When changing behavior, update or add tests.

## Review Checks

- Flag broad `except Exception` usage unless there is a clear reason.
- Flag side effects introduced at import time.
- Flag behavior changes that do not include corresponding test updates.
- Flag use of `print` for runtime diagnostics where `logging` is more appropriate.
