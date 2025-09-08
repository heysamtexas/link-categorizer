# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

**Testing:**
- Run all tests: `uv run pytest tests/`
- Run specific test: `uv run pytest tests/link_categorizer_test.py::test_categorize_link_domains -v`
- Run with coverage: `uv run pytest --cov=src/link_categorizer tests/`

**Code Quality:**
- Lint: `uv run ruff check src/`
- Format: `uv run ruff format src/`
- Type check: `uv run mypy src/`
- Security scan: `uv run bandit -r src/`
- All pre-commit hooks: `uv run pre-commit run --all-files`

**Development setup:**
```bash
uv pip install -e .[dev]
```

## Release Process

**Creating a new release:**
1. Update version in `pyproject.toml`
2. Commit changes: `git commit -am "feat: description of changes"`
3. Create and push tag: `git tag v0.1.x && git push origin v0.1.x`
4. GitHub Actions automatically:
   - Runs all tests and quality checks
   - Builds the package with `uv build`
   - Publishes to PyPI using trusted publishing

**Note:** All building and publishing is handled by the GitHub Actions workflow in `.github/workflows/publish.yml`.

## Architecture Overview

This is a Python library that categorizes HTML anchor tags based on domain patterns, URL paths, and text content. The core architecture consists of:

**Main categorization logic** (`src/link_categorizer/categorizer.py`):
- `categorize_link()`: Single link categorization
- `categorize_links()`: Batch processing with deduplication
- Three-tier matching system: domain patterns → path patterns → text patterns
- Falls back to "unknown" category if no matches found

**Pattern matching system:**
- `DOMAIN_MATCHERS`: Regex patterns for domains (e.g., github.com → "code repository")
- `PATH_MATCHERS`: URL path patterns (e.g., /about → "about")
- `TEXT_MATCHERS`: Link text patterns (e.g., "documentation" → "documentation")

**Key behavior:**
- Link deduplication based on normalized URLs
- Text analysis limited to first 50 characters (`TEXT_MAX_LENGTH`)
- Domain matching takes precedence over path/text matching
- Returns `None` for ignored links (empty href, mailto:, tel:, etc.)

**Project structure:**
- `src/link_categorizer/`: Main package code
- `tests/`: Unit tests using pytest
- Modern Python packaging with hatchling and pyproject.toml
- CI/CD with GitHub Actions testing Python 3.8-3.12

## Code Style

- Python 3.8+ with type hints required
- Ruff formatting, 120 character line length
- Uses `dict`, `list`, `str | None` type syntax
- snake_case naming convention
- Import order: standard library, third-party, local
