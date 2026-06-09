---
title: Package Management with uv
impact: MEDIUM
impactDescription: Fast, reproducible project workflow
tags: [uv, package-management, pyproject.toml, dependency-groups, lock-file]
---

# Package Management with uv [MEDIUM]

## Description
uv is a Python package and project manager from Astral. Prefer its project workflow for new projects: declare dependencies in `pyproject.toml`, keep the resolved `uv.lock` in version control, sync environments with `uv sync`, and run commands with `uv run`. Use `uv pip ...` mainly for existing `requirements.txt` or pip-compatible workflows.

## Bad Example
```bash
# Ad hoc installs are hard to reproduce.
python -m venv .venv
. .venv/bin/activate
pip install fastapi pytest ruff
pytest
```

```toml
# pyproject.toml has no source of truth for dependencies.
[project]
name = "myproject"
version = "0.1.0"
dependencies = []
```

## Good Example
```bash
# Scaffold a project.
uv init

# Add dependencies. uv updates pyproject.toml and the lockfile.
uv add fastapi pydantic
uv add --dev pytest ruff mypy
uv remove pydantic

# Sync the environment from uv.lock.
uv sync
uv sync --locked --no-dev

# Run commands inside the project environment.
uv run pytest
uv run python -m myapp

# Upgrade locked dependencies intentionally.
uv lock --upgrade
uv lock --upgrade-package fastapi
```

```toml
[project]
name = "myproject"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = [
    "fastapi>=0.115",
    "pydantic>=2.0",
]

[dependency-groups]
dev = [
    "mypy>=1.13",
    "pytest>=8.0",
    "ruff>=0.8",
]
```

## Project workflow
| Command | Purpose |
|---------|---------|
| `uv init` | Scaffold a project |
| `uv add` / `uv remove` | Edit dependencies in `pyproject.toml` |
| `uv add --dev` | Add to the `dev` dependency group |
| `uv lock` | Resolve dependencies into `uv.lock` |
| `uv sync` | Make `.venv` match the lockfile |
| `uv run` | Run a command in the project environment |
| `uvx` | Run a tool without adding it to the project |

## Environment usage
| Environment | Command |
|-------------|---------|
| Development | `uv sync` |
| CI | `uv sync --locked` |
| Production | `uv sync --locked --no-dev` |

## Dependency groups
```bash
uv add --dev pytest ruff mypy
uv sync --no-dev
uv sync --group docs
uv sync --all-groups
```

## pip-compatible mode
For existing `requirements.txt` projects, uv also supports a pip-compatible interface:

```bash
uv venv
uv pip install -e ".[dev]"
uv pip compile pyproject.toml -o requirements.lock --generate-hashes
uv pip sync requirements.lock
```

For new projects, prefer the project workflow above. `uv.lock` is maintained by commands such as `uv add`, `uv lock`, `uv sync`, and `uv run`.

## Notes
- `uv run` verifies the lockfile and environment before running commands
- `uv sync` is exact by default and removes packages not present in the lockfile
- The `dev` dependency group is included by default; use `--no-dev` to exclude it
- Commit `uv.lock` for project workflow; never commit `.venv/`
- Use `uv sync --locked` in CI to fail when `uv.lock` is stale
- Use `uv sync --frozen` only when intentionally skipping freshness checks

## References
- [uv Documentation](https://docs.astral.sh/uv/)
- [uv - Working on projects](https://docs.astral.sh/uv/guides/projects/)
- [uv - Locking and syncing](https://docs.astral.sh/uv/concepts/projects/sync/)
- [uv - Managing dependencies](https://docs.astral.sh/uv/concepts/projects/dependencies/)
