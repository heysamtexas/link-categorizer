# AGENTS.md

## Build/Lint/Test Commands
- Run all tests: `uv run pytest tests/`
- Run single test: `uv run pytest tests/link_categorizer_test.py::test_categorize_link_domains -v`
- Lint: `uv run ruff check src/`
- Format: `uv run ruff format src/`
- Type check: `uv run mypy src/`
- Security scan: `uv run bandit -r src/`
- Pre-commit: `uv run pre-commit run --all-files`
- Build: `uv run hatch build`

## Code Style
- **Python**: 3.7+, type hints required (`py.typed` present)
- **Formatting**: Ruff, line-length 120
- **Imports**: Standard library first, then third-party
- **Naming**: snake_case for functions/variables, UPPER_CASE for constants
- **Error handling**: Return None for ignored links, explicit categories for others
- **Types**: Use `dict`, `list`, `str | None` syntax
- **Constants**: TEXT_MAX_LENGTH=50, DOMAIN_MATCHERS, PATH_MATCHERS, TEXT_MATCHERS
