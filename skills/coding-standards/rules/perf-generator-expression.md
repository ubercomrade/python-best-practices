---
title: Use Generators for Large Datasets
impact: CRITICAL
impactDescription: Lower memory use for lazy iteration
tags: [performance, memory, generator, lazy-evaluation]
---

# Use Generators for Large Datasets [CRITICAL]

## Description
When processing large or streaming datasets, lists hold all elements in memory while generators produce elements one at a time. This is especially useful when iterating results once or when intermediate results are not needed.

## Bad Example
```python
# Stores every result before the caller can consume it
def get_large_data() -> list[dict[str, int]]:
    return [{"id": i, "value": i * 2} for i in range(1_000_000)]

# Unnecessary intermediate list
total = sum([x * x for x in range(1_000_000)])
```

## Good Example
```python
from collections.abc import Iterator

# Lazy evaluation with generator
def get_large_data() -> Iterator[dict[str, int]]:
    for i in range(1_000_000):
        yield {"id": i, "value": i * 2}

# Generator expression: just change brackets to parentheses
total = sum(x * x for x in range(1_000_000))
```

## Notes
- Functions like `sum()`, `max()`, `min()`, `any()`, `all()` accept generators directly
- Use lists when you need to iterate multiple times (generators can only be consumed once)
- File line processing is generator-like with `for line in file:`
- For small collections, readability usually matters more than memory differences

## References
- [PEP 289 - Generator Expressions](https://peps.python.org/pep-0289/)
