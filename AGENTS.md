# AGENTS.md

This file is intended to help contributors, especially those using AI assistants, quickly understand repository expectations, workflows, and how to validate changes.

## Purpose and Scope

- Provide a single entry point for common development tasks.
- Summarize test commands, required checks, and definition of done.
- Link to deeper documentation.

## Project Overview

Manim Slides is a tool for live presentations using Manim (community edition) or ManimGL. It provides:

- `Slide` / `ThreeDSlide` base classes for rendering animations into slide-based presentations.
- CLI `manim-slides` for presenting, converting (HTML, PDF, PPTX, ZIP), and wizard configuration.
- Qt-based player and RevealJS HTML template.

Key directories:

- `manim_slides/slide/` – Slide base classes for Manim and ManimGL.
- `manim_slides/present/` – Qt player and presentation logic.
- `manim_slides/convert.py` – Converters (HTML, PDF, PPTX, ZIP).
- `manim_slides/config.py` – Configuration models.
- `manim_slides/utils.py` – Video concatenation and reversing utilities.
- `tests/` – Pytest suite.

## Required Checks Before Opening a PR

1. **Formatting & Linting**: `pre-commit` hooks must pass.
   - Install: `pre-commit install`
   - Run: `pre-commit run --all-files`
   - Includes: `ruff` (lint + format), `ty` (type checking), `codespell`, and repo-specific hooks.

2. **Tests**: Ensure relevant tests pass.
   - Full suite (requires Qt offscreen): `uv run --python 3.11 --frozen --group tests --no-dev pytest`
   - Without Qt (lighter, CI-friendly): `uv run --python 3.11 --frozen --group tests-no-qt --no-dev pytest`
   - Single file: `uv run pytest tests/test_config.py -v`

3. **Documentation**: If you add or modify public functions/methods, add docstrings and type hints. Update docs in `docs/` if behavior changes.

4. **CI**: All GitHub Actions workflows must be green (pip-install matrix + pytest + markdown-link-check).

## Test Expectations

- Minimum: `tests-no-qt` group should pass locally (no display required, uses `QT_QPA_PLATFORM=offscreen`).
- Full: `tests` group includes Qt tests (needs `pyside6` or `pyqt6`).
- Coverage is tracked via `codecov`; aim to maintain or improve coverage.
- If you fix a bug, add or update a regression test in `tests/`.

## Typical Workflows

- **Render & Present**: `manim-slides render example.py BasicExample` then `manim-slides BasicExample`
- **Convert**: `manim-slides convert BasicExample slides.html --one-file` or `--to=pptx`
- **Check Health**: `manim-slides checkhealth`
- **Wizard**: `manim-slides wizard` or `manim-slides init`

## Coding Conventions

- Python >=3.10, use type hints.
- Follow `ruff` rules (line length 88, isort, etc.).
- Use `logger` from `manim_slides.logger` instead of print for CLI.
- Keep `manim_slides/resources.py` excluded from lint (generated file).

## Where to Find Deeper Docs

- README: `README.md`
- Contributing guide: `docs/source/contributing/index.md` and `workflow.md`
- Internals: `docs/source/contributing/internals.md`
- CLI reference: `manim-slides --help` and `docs/source/reference/cli.md`
- CI workflows: `.github/workflows/tests.yml`

## Definition of Done

- Code formatted and linted, type checks pass.
- Tests added/updated and passing.
- Documentation updated if needed.
- PR title is a short, conventional description (e.g., `fix(present): ...`, `feat(convert): ...`).
- No merge conflicts with `main`, CI green.

## AI Assistant Tips

- When fixing bugs, first reproduce with minimal example or existing test.
- Prefer small, focused PRs over large refactors.
- Avoid duplicating open PRs – check `gh pr list --repo jeertmans/manim-slides --state open`.
- For Qt-related changes, test with `QT_QPA_PLATFORM=offscreen` if no display.

## Links

- Main repo: https://github.com/jeertmans/manim-slides
- Docs: https://eertmans.be/manim-slides/
- Issues: https://github.com/jeertmans/manim-slides/issues
- Changelog: `CHANGELOG.md`
