---
name: coding-standards
description: Use for Python code review, refactoring, async Python, performance guidance, maintainability, pragmatic functional style, numerical Python with NumPy/Numba, minimizing unnecessary OOP, and generating idiomatic Python code with practical examples.
globs:
  - "**/*.py"
  - "pyproject.toml"
  - "setup.py"
  - "requirements*.txt"
---

# Python Coding Standards

A comprehensive collection of Python coding standards and best practices. Designed for AI agents and LLMs to generate high-quality, performant, and maintainable Python code using pragmatic functional design, appropriate OOP, and practical numerical Python patterns.

## Categories

### Performance Optimization [CRITICAL]
Apply Python and numerical optimization patterns to improve processing speed and memory efficiency where they fit the workload.

| Rule | Description |
|------|-------------|
| [perf-list-comprehension](rules/perf-list-comprehension.md) | Prefer list comprehensions for clear list construction |
| [perf-generator-expression](rules/perf-generator-expression.md) | Use generators for large or single-pass datasets |
| [perf-numpy-vectorization](rules/perf-numpy-vectorization.md) | Prefer readable NumPy vectorization for array-heavy code |
| [perf-numba-kernels](rules/perf-numba-kernels.md) | Use Numba for hot numerical kernels |
| [perf-dict-get](rules/perf-dict-get.md) | Use dict.get() for efficient default values |
| [perf-set-lookup](rules/perf-set-lookup.md) | Use set for repeated membership checks |
| [perf-str-join](rules/perf-str-join.md) | Use join for string building from many parts |

### Async Processing [HIGH]
Efficient asynchronous programming patterns using asyncio.

| Rule | Description |
|------|-------------|
| [async-gather](rules/async-gather.md) | Use asyncio.gather for independent tasks |
| [async-create-task](rules/async-create-task.md) | Proper background task creation |
| [async-context-manager](rules/async-context-manager.md) | Resource management with async with |
| [async-semaphore](rules/async-semaphore.md) | Limit concurrency with semaphores |

### Design Principles [HIGH]
Software design principles for maintainability, explicit data flow, pragmatic functional style, and extensibility.

| Rule | Description |
|------|-------------|
| [design-philosophy](rules/design-philosophy.md) | DRY, YAGNI, KISS principles |
| [design-single-responsibility](rules/design-single-responsibility.md) | Single Responsibility Principle |
| [design-dependency-injection](rules/design-dependency-injection.md) | Loose coupling with dependency injection |
| [design-pure-functions](rules/design-pure-functions.md) | Prefer pure functions without side effects |
| [design-functional-core-shell](rules/design-functional-core-shell.md) | Separate functional core from imperative shell |
| [design-explicit-data-flow](rules/design-explicit-data-flow.md) | Make inputs, outputs, and config explicit |
| [design-immutable-config-results](rules/design-immutable-config-results.md) | Use immutable config and result objects |
| [design-functional-pipeline](rules/design-functional-pipeline.md) | Use simple functional pipelines for orchestration |
| [design-singledispatch](rules/design-singledispatch.md) | Use singledispatch for type-based operations |
| [design-avoid-functional-overabstraction](rules/design-avoid-functional-overabstraction.md) | Avoid heavy FP abstractions without project need |
| [design-early-return](rules/design-early-return.md) | Reduce nesting with early returns |

### Object-Oriented Programming [MEDIUM]
Best practices for Pythonic object-oriented programming.

| Rule | Description |
|------|-------------|
| [oop-composition-over-inheritance](rules/oop-composition-over-inheritance.md) | Prefer composition over inheritance |
| [oop-dataclass](rules/oop-dataclass.md) | Use dataclass for data containers |
| [oop-protocol](rules/oop-protocol.md) | Prefer Protocol over abstract base classes |
| [oop-property](rules/oop-property.md) | Use property instead of getters |

## Quick Reference

### Performance Patterns
```python
# List comprehension (not loops)
result = [x * 2 for x in items]

# Generator for large data
total = sum(x * x for x in range(1_000_000))

# dict.get() with default
value = config.get("key", default_value)

# Set for fast lookup
valid_ids: set[int] = {1, 2, 3}
if item_id in valid_ids: ...

# join for strings
result = ",".join(values)

# NumPy broadcasting for arrays
log_odds = np.log2(pwm / background[:, None])

# Numba for hot numerical kernels
@njit(cache=True)
def score_window(encoded: np.ndarray, pwm: np.ndarray) -> float:
    score = 0.0
    for pos in range(encoded.shape[0]):
        base = encoded[pos]
        score += pwm[base, pos]
    return score
```

### Layered Architecture
```text
I/O shell
  parse CLI, read files, log, save outputs

Functional orchestration
  validate config, compose transformations, dispatch by model type

Numerical kernels
  NumPy vectorization or small Numba functions
```

### Async Patterns
```python
# Concurrent execution
results = await asyncio.gather(task1(), task2(), task3())

# Resource management
async with aiohttp.ClientSession() as session:
    async with session.get(url) as response:
        data = await response.json()

# Concurrency limit
semaphore = asyncio.Semaphore(10)
async with semaphore:
    await do_work()
```

### Design Patterns
```python
# Pure core function
def center_scores(scores: np.ndarray) -> np.ndarray:
    return scores - scores.mean()

# Dependency injection
class Service:
    def __init__(self, repository: Repository) -> None:
        self.repository = repository

# Early return
def process(data: Data | None) -> Result:
    if data is None:
        return Result.empty()
    # main logic here
```

### OOP Patterns
Use classes when there is durable state, identity, a protocol/interface, or a data container. Start with functions and explicit data when a class would only wrap one operation.

```python
# Dataclass
@dataclass
class User:
    name: str
    email: str

# Protocol for interfaces
class Repository(Protocol):
    def get(self, id: str) -> Entity: ...

# Property
@property
def full_name(self) -> str:
    return f"{self.first} {self.last}"
```

## See Also

- [Python Tooling](../tooling/SKILL.md) - Tools to enforce these standards automatically (ruff, mypy, pytest, pyscn, uv)
