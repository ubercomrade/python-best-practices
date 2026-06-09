# Section Definitions

## Performance Optimization (perf)
**Impact:** CRITICAL

Apply Python and numerical optimization patterns to improve processing speed and memory efficiency where they fit the workload.
Covers list comprehensions, generators, efficient data structure selection, NumPy vectorization, Numba kernels, and other techniques that leverage Python's characteristics.

**Rules:**
- `perf-list-comprehension` - Prefer list comprehensions for clear list construction
- `perf-generator-expression` - Use generators for large or single-pass datasets
- `perf-numpy-vectorization` - Prefer readable NumPy vectorization for array-heavy code
- `perf-numba-kernels` - Use Numba for hot numerical kernels
- `perf-dict-get` - Use dict.get() for efficient default values
- `perf-set-lookup` - Use set for repeated membership checks
- `perf-str-join` - Use join for string building from many parts

## Async Processing (async)
**Impact:** HIGH

Efficient asynchronous programming patterns using asyncio.
Covers concurrent execution of I/O-bound operations, resource management, and concurrency control.

**Rules:**
- `async-gather` - Use asyncio.gather for independent tasks
- `async-create-task` - Proper background task creation
- `async-context-manager` - Resource management with async with
- `async-semaphore` - Limit concurrency with semaphores

## Design Principles (design)
**Impact:** HIGH

Software design principles for maintainability, explicit data flow, pragmatic functional style, and extensibility.
Covers single responsibility, dependency injection, pure functions, functional core/shell boundaries, and other principles for building robust codebases.

**Rules:**
- `design-philosophy` - DRY, YAGNI, KISS principles
- `design-single-responsibility` - Single Responsibility Principle
- `design-dependency-injection` - Loose coupling with dependency injection
- `design-pure-functions` - Prefer pure functions without side effects
- `design-functional-core-shell` - Separate functional core from imperative shell
- `design-explicit-data-flow` - Make inputs, outputs, and config explicit
- `design-immutable-config-results` - Use immutable config and result objects
- `design-functional-pipeline` - Use simple functional pipelines for orchestration
- `design-singledispatch` - Use singledispatch for type-based operations
- `design-avoid-functional-overabstraction` - Avoid heavy FP abstractions without project need
- `design-early-return` - Reduce nesting with early returns

## Object-Oriented Programming (oop)
**Impact:** MEDIUM

Best practices for Pythonic object-oriented programming.
Covers class design, composition vs inheritance, dataclasses, and protocols.

**Rules:**
- `oop-composition-over-inheritance` - Prefer composition over inheritance
- `oop-dataclass` - Use dataclass for data containers
- `oop-protocol` - Prefer Protocol over abstract base classes
- `oop-property` - Use property instead of getters
