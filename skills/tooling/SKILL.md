---
name: tooling
description: Use for Python tooling setup, pyproject.toml configuration, Ruff linting and formatting, mypy type checking, pytest configuration, uv project workflows, lockfiles, and dependency groups.
globs:
  - "**/*.py"
  - "pyproject.toml"
  - "mypy.ini"
  - "ruff.toml"
---

# Python Tooling

A comprehensive guide to Python development tools. Configuration best practices for analysis, linters, type checkers, formatters, test frameworks, and package managers.

## Why These Tools Matter

To write high-quality Python code, we recommend adopting these tools:

- **pyscn** - Detect dead code, duplicates, and circular dependencies to prevent technical debt
- **ruff** - Catch bugs and style violations early with fast static analysis
- **mypy** - Find errors before runtime with type checking, also improves IDE completion
- **pytest** - Build confidence in changes with reliable tests
- **uv** - Improve developer experience with fast dependency management

Integrating these into CI/CD reduces code review burden and maintains consistent quality.

## Categories

### Analysis [HIGH]
Structural code analysis for quality assessment.

| Rule | Description |
|------|-------------|
| [analysis-pyscn](rules/analysis-pyscn.md) | Dead code, clones, dependencies, complexity with pyscn |

### Linting [CRITICAL]
Static code analysis with ruff for consistent, high-quality code.

| Rule | Description |
|------|-------------|
| [lint-ruff](rules/lint-ruff.md) | Fast, comprehensive linting with ruff |

### Type Checking [HIGH]
Static type checking with mypy for type safety and better IDE support.

| Rule | Description |
|------|-------------|
| [type-mypy](rules/type-mypy.md) | Static type checking with mypy |

### Formatting [HIGH]
Consistent code formatting with `ruff format`; use `ruff check --select=I --fix` for import sorting.

| Rule | Description |
|------|-------------|
| [fmt-ruff](rules/fmt-ruff.md) | Code formatting with ruff and explicit import sorting workflow |

### Testing [HIGH]
Test framework configuration with pytest for reliable testing.

| Rule | Description |
|------|-------------|
| [test-pytest](rules/test-pytest.md) | Testing with pytest, fixtures, and coverage |

### Package Management [MEDIUM]
Modern Python packaging with uv, pyproject.toml, dependency groups, and lockfiles.

| Rule | Description |
|------|-------------|
| [pkg-uv](rules/pkg-uv.md) | Project workflow with uv |
| [pkg-pyproject](rules/pkg-pyproject.md) | Project configuration with pyproject.toml and dependency groups |

## Quick Reference

### Minimal pyproject.toml
```toml
[project]
name = "myproject"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = []

[dependency-groups]
dev = ["ruff", "mypy", "pytest", "pytest-cov", "pyscn"]

[tool.ruff]
target-version = "py311"
line-length = 88

[tool.ruff.lint]
select = ["E", "F", "W", "I", "UP", "B", "SIM", "PTH"]

[tool.mypy]
python_version = "3.11"
strict = true

[tool.pytest.ini_options]
testpaths = ["tests"]
addopts = ["-v", "--tb=short"]

[tool.pyscn]
max_complexity = 15
```

### Common Commands
```bash
# Analysis
pyscn analyze .                 # Full quality analysis
pyscn check .                   # CI quality gate

# Linting
ruff check .                    # Check for issues
ruff check . --fix              # Auto-fix issues

# Formatting
ruff format .                   # Format code
ruff check . --select=I --fix   # Sort imports

# Type checking
mypy .                          # Type check

# Testing
pytest                          # Run tests
pytest --cov=src                # With coverage

# Package management (uv)
uv add fastapi                  # Add runtime dependency
uv add --dev ruff mypy pytest   # Add dev dependency group
uv sync                         # Sync .venv from uv.lock
uv run pytest                   # Run inside the project environment
```
