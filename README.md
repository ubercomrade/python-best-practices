# python-best-practices

Python best-practice skills for AI coding agents. This repository contains multiple installable skills for writing, reviewing, refactoring, and tooling Python projects.

## Installation

```bash
uvx add-skills ludo-technologies/python-best-practices
```

## Skills

### coding-standards

Python coding standards and best practices. 26 rules across 4 categories, including pragmatic functional design and numerical Python guidance with NumPy and Numba.

| Category | Impact | Rules |
|----------|--------|-------|
| **Performance Optimization** | CRITICAL | list comprehension, generator expression, NumPy vectorization, Numba kernels, dict.get(), set lookup, str.join() |
| **Async Processing** | HIGH | asyncio.gather, create_task, async context manager, semaphore |
| **Design Principles** | HIGH | DRY/YAGNI/KISS, single responsibility, dependency injection, pure functions, functional core/shell, explicit data flow, immutable config/results, functional pipelines, singledispatch, avoiding FP overabstraction, early return |
| **Object-Oriented Programming** | MEDIUM | composition over inheritance, dataclass, Protocol, property; use classes for state, identity, protocols, or data containers rather than by default |

The recommended architecture separates I/O shell, functional orchestration, and numerical kernels:

```text
I/O shell
  parse CLI, read files, log, save outputs

Functional orchestration
  validate config, compose transformations, dispatch by model type

Numerical kernels
  NumPy vectorization or small Numba functions
```

### tooling

Python development tooling configuration. 7 rules across 6 categories.

| Category | Impact | Tools |
|----------|--------|-------|
| **Analysis** | HIGH | pyscn (dead code, clones, complexity) |
| **Linting** | CRITICAL | ruff |
| **Type Checking** | HIGH | mypy |
| **Formatting** | HIGH | ruff format |
| **Testing** | HIGH | pytest |
| **Package Management** | MEDIUM | uv, pyproject.toml |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on adding new rules.

## License

MIT
