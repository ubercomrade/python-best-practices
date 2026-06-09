---
title: Formatting with Ruff
impact: HIGH
impactDescription: Consistent formatter output
tags: [ruff, formatting, black, isort]
---

# Formatting with Ruff [HIGH]

## Description
Ruff's formatter is designed as a drop-in replacement for Black and is available as `ruff format`. It formats code but does not sort imports; use `ruff check --select=I --fix` for import sorting.

## Bad Example
```toml
[tool.ruff.format]
quote-style = "single"

[tool.ruff.lint.isort]
# These non-default isort settings can conflict with formatter output.
force-single-line = true
lines-after-imports = 1
```

```bash
# This formats code but does not sort imports.
ruff format .
```

## Good Example
```toml
# pyproject.toml
[tool.ruff]
line-length = 88
target-version = "py311"

[tool.ruff.format]
quote-style = "double"
indent-style = "space"
skip-magic-trailing-comma = false
line-ending = "auto"

exclude = [
    "migrations/",
    "*.pyi",
]

[tool.ruff.lint]
select = ["I"]  # Enable isort rules

[tool.ruff.lint.isort]
known-first-party = ["myproject"]
```

```bash
ruff check . --select=I --fix
ruff format .
```

## Import order
```python
from __future__ import annotations

import os
import sys
from pathlib import Path
from typing import TYPE_CHECKING

from fastapi import FastAPI
from pydantic import BaseModel

from myproject.core import service
from myproject.utils import helper

if TYPE_CHECKING:
    from myproject.models import User
```

## Line length
```toml
[tool.ruff]
line-length = 88

[tool.ruff.lint.pycodestyle]
max-line-length = 88
```

| Length | Rationale |
|--------|-----------|
| 79 | PEP 8 original |
| 88 | Black/Ruff default |
| 100 | More horizontal space |
| 120 | Wide monitors |

## Commands
```bash
ruff format .                  # Format all files
ruff format . --check          # Check without modifying
ruff format . --diff           # Show diff
ruff check . --select=I --fix  # Fix import order
```

## Notes
- Ruff format follows Black-style defaults but has documented deviations
- Use the same `line-length` for linting and formatting
- Keep import sorting in lint configuration (`I`) and run it before formatting
- `known-first-party` is usually auto-detected, but explicit configuration can help monorepos
- Avoid non-default isort settings that Ruff documents as formatter-incompatible

## References
- [Ruff Formatter](https://docs.astral.sh/ruff/formatter/)
- [Ruff isort Rules](https://docs.astral.sh/ruff/rules/#isort-i)
