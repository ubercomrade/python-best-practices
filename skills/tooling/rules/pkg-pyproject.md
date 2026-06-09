---
title: Project Configuration with pyproject.toml
impact: MEDIUM
impactDescription: Unified project and tool configuration
tags: [pyproject.toml, packaging, pep621, pep735, dependency-groups]
---

# Project Configuration with pyproject.toml [MEDIUM]

## Description
`pyproject.toml` is the standard configuration file for modern Python projects. Use `[project]` for package metadata and runtime dependencies, `[dependency-groups]` for local development dependencies, `[build-system]` for build requirements, and `[tool.*]` for tool configuration.

## Bad Example
```toml
# Development tools are exposed as installable package extras.
[project.optional-dependencies]
dev = ["ruff", "mypy", "pytest"]

# Tool configuration is incomplete.
[tool.ruff]
line-length = 88
```

```bash
# Installs dev tools through package extras instead of dependency groups.
uv pip install -e ".[dev]"
```

## Good Example
```toml
[project]
name = "myproject"
version = "0.1.0"
description = "A Python project"
readme = "README.md"
license = {text = "MIT"}
requires-python = ">=3.11"
authors = [
    {name = "Your Name", email = "you@example.com"},
]
classifiers = [
    "Development Status :: 4 - Beta",
    "Programming Language :: Python :: 3.11",
    "Programming Language :: Python :: 3.12",
]
dependencies = [
    "fastapi>=0.115",
    "pydantic>=2.0",
]

[project.optional-dependencies]
postgres = [
    "psycopg[binary]>=3.2",
]
docs = [
    "mkdocs>=1.6",
]

[dependency-groups]
dev = [
    "ruff>=0.8.0",
    "mypy>=1.13.0",
    "pytest>=8.0",
    "pytest-cov>=5.0",
]

[project.scripts]
myproject = "myproject.cli:main"

[project.urls]
Homepage = "https://github.com/you/myproject"
Documentation = "https://myproject.readthedocs.io"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.ruff]
target-version = "py311"
line-length = 88

[tool.ruff.lint]
select = ["E", "F", "W", "I", "UP", "B", "SIM", "PTH", "RUF"]

[tool.mypy]
python_version = "3.11"
strict = true

[tool.pytest.ini_options]
testpaths = ["tests"]
addopts = ["-v", "--tb=short"]

[tool.pyscn]
max_complexity = 15
```

## Dependency model
```toml
# Runtime dependencies published with the package.
[project]
dependencies = [
    "fastapi>=0.115",
    "pydantic>=2.0",
]

# Optional runtime extras published with the package.
[project.optional-dependencies]
postgres = ["psycopg[binary]>=3.2"]

# Local development dependencies not published as package extras.
[dependency-groups]
dev = ["ruff>=0.8.0", "mypy>=1.13.0", "pytest>=8.0"]
docs = ["mkdocs>=1.6"]
all = [{include-group = "dev"}, {include-group = "docs"}]
```

```bash
uv add fastapi
uv add --optional postgres "psycopg[binary]"
uv add --dev ruff mypy pytest
uv sync --group docs
```

## Key Sections
| Section | Purpose |
|---------|---------|
| `[project]` | Package metadata and runtime dependencies |
| `[project.optional-dependencies]` | Published package extras |
| `[dependency-groups]` | Local dependency groups for development, tests, docs, and tooling |
| `[project.scripts]` | CLI entry points |
| `[build-system]` | Build backend |
| `[tool.*]` | Tool configurations |

## Notes
- `[project]` follows PEP 621
- `[dependency-groups]` follows PEP 735 and is suitable for linting, tests, docs, and non-package projects
- Use extras for optional runtime features that users install from your package
- Use dependency groups for local workflows that should not become package metadata
- Use lower bounds for compatibility and rely on the lockfile for exact resolved versions
- Keep tool configs in `[tool.xxx]` sections

## References
- [PEP 621](https://peps.python.org/pep-0621/)
- [PEP 735](https://peps.python.org/pep-0735/)
- [Python Packaging Guide](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/)
- [Dependency Groups specification](https://packaging.python.org/en/latest/specifications/dependency-groups/)
