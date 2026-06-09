---
title: Linting with Ruff
impact: CRITICAL
impactDescription: Fast, comprehensive linting
tags: [ruff, linting, pyproject.toml, configuration]
---

# Linting with Ruff [CRITICAL]

## Description
Ruff is a fast Python linter written in Rust. Use `ruff check` for linting, import sorting, and safe auto-fixes. Keep formatting in `ruff format` so lint and formatting responsibilities remain explicit.

## Bad Example
```toml
[tool.ruff]
# Missing target-version means pyupgrade and parser behavior may not match the project.
line-length = 88

[tool.ruff.lint]
# ALL is noisy for most projects and can enable conflicting or preview-heavy rules.
select = ["ALL"]
ignore = ["F401", "F841"]
```

```bash
# Unsafe fixes without review can change behavior.
ruff check . --fix --unsafe-fixes
```

## Good Example
```toml
# pyproject.toml
[tool.ruff]
target-version = "py311"
line-length = 88
exclude = [
    ".venv",
    "migrations",
    "__pycache__",
]

[tool.ruff.lint]
select = [
    "E",      # pycodestyle errors
    "F",      # Pyflakes
    "W",      # pycodestyle warnings
    "I",      # isort
    "UP",     # pyupgrade
    "B",      # flake8-bugbear
    "SIM",    # flake8-simplify
    "PTH",    # flake8-use-pathlib
    "RUF",    # Ruff-specific rules
]
ignore = []

[tool.ruff.lint.per-file-ignores]
"tests/**/*.py" = [
    "S101",    # assert is fine in tests
    "ARG001",  # unused fixtures
    "PLR2004", # magic values ok
]
"__init__.py" = [
    "F401",    # re-exported imports
]
"scripts/**/*.py" = [
    "T201",    # print allowed
]
```

## Auto-fix policy
```toml
[tool.ruff.lint]
fixable = ["ALL"]
unfixable = [
    "F841",   # unused variable - might be intentional
    "ERA001", # commented code - might be needed
]
```

## Commands
```bash
ruff check .                       # Check for issues
ruff check . --fix                 # Auto-fix safe issues
ruff check . --fix --unsafe-fixes  # Include unsafe fixes
ruff check . --select=I --fix      # Sort imports
ruff check . --select=ALL          # Check all rules
ruff check . --statistics          # Show rule counts
```

## Rule categories
| Prefix | Name | Purpose |
|--------|------|---------|
| E/W | pycodestyle | PEP 8 style |
| F | Pyflakes | Logic errors |
| I | isort | Import sorting |
| UP | pyupgrade | Modernize syntax |
| B | flake8-bugbear | Bug patterns |
| SIM | flake8-simplify | Simplifications |
| PTH | flake8-use-pathlib | Path handling |
| RUF | Ruff | Ruff-specific |

## Notes
- Always specify `target-version` to match your project's minimum Python
- Use `ruff check --select=I --fix` for import sorting; `ruff format` does not sort imports
- Use `--fix` for safe auto-fixes, review `--unsafe-fixes` before applying
- Start with recommended rules, then add more as the codebase matures
- Use `per-file-ignores` instead of global `ignore` when possible

## References
- [Ruff Documentation](https://docs.astral.sh/ruff/)
- [Ruff Rules](https://docs.astral.sh/ruff/rules/)
- [Ruff Settings](https://docs.astral.sh/ruff/settings/)
