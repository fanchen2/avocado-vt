# GitHub Copilot Instructions for avocado-vt

## Project Overview

This repository is the Avocado VT plugin, providing virtualization test support
on top of the Avocado framework.

Key areas:

- `avocado_vt/`: plugin entry points, discovery, loader, options, utilities
- `virttest/`: core virtualization test libraries and helpers
- `docs/`: Sphinx documentation and contributor guides
- `selftests/`: unit and functional selftests
- `examples/`: sample tests and usage patterns

Changes here often affect many downstream test providers. Prefer small,
well-scoped changes that preserve backward compatibility unless the task
explicitly calls for behavior changes.

## General Working Rules

- Follow the repository's existing abstractions instead of duplicating logic in tests.
- Preserve public APIs, parameter names, and expected behaviors in `virttest/` when possible.
- Avoid broad refactors across `virttest/` unless the task clearly justifies them.
- When fixing bugs, prefer the lowest layer that correctly addresses the issue.
- Keep changes aligned with Avocado and virttest conventions already present nearby.

## Python Style

- Follow the coding style enforced by `black`.
- Prefer descriptive snake_case names and simple, readable control flow.
- Keep functions focused and reasonably short.
- Use `is None` and `is not None` for `None` checks.
- Avoid wildcard imports.

### Import Order

Use this import grouping order:

1. Standard library
2. Third-party modules
3. Avocado modules
4. Avocado-VT / `virttest` modules

Within a group, regular imports should appear before `from ... import ...` imports.

## Docstrings And Comments

- Add docstrings for non-trivial functions and methods.
- Prefer reStructuredText-friendly docstrings because documentation is Sphinx-based.
- Comments should explain intent, not restate obvious mechanics.

## Repository-Specific Expectations

### Core Libraries

- `virttest/` is shared infrastructure. Be conservative with behavior changes.
- Reuse existing helpers from `virttest` instead of adding parallel implementations.
- Keep layering clean: lower-level helpers should not depend on higher-level test utilities.

### Plugin Code

- In `avocado_vt/`, preserve plugin contracts, CLI behavior, loader behavior, and option names.
- Avoid changing discovery or loader semantics unless the task specifically requires it.

### Tests And Selftests

- When adding tests, match the surrounding test style and fixtures.
- Prefer targeted selftests for new library behavior when practical.
- Do not touch large selftest data files unless required by the change.

### Documentation

- Update docs when changing user-visible behavior, CLI options, or contributor workflows.
- Keep examples and docs consistent with the actual code paths and command names.

## Validation

- Preferred validation is `pre-commit run --all-files` or a narrower relevant subset when appropriate.
- Prefer targeted checks first (for example `pre-commit run --files <touched_file>` and related selftests), then run broader validation only when needed.
- Run targeted tests or selftests when the affected area has local coverage.
- If a full validation run is too expensive or unavailable, state what was not verified.

## Editing Priorities

When making changes, prefer this order:

1. Fix the requested behavior at the right layer.
2. Preserve compatibility for downstream users and test providers.
3. Keep diffs reviewable and localized.
4. Update docs or selftests when behavior changes.

Avoid:

- style-only churn across shared library code
- unnecessary renames in public helpers
- duplicating existing `virttest` functionality
- changing CLI or plugin behavior without calling it out

## Recommended Copilot Behavior

When assisting in this repository, Copilot should:

- identify whether the change belongs in `avocado_vt/`, `virttest/`, docs, or selftests
- preserve `virttest` API stability unless explicitly asked to change it
- follow Black formatting and the documented import order
- use Sphinx-friendly docstrings for non-trivial new code
- validate with pre-commit or targeted tests when feasible