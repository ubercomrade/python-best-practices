---
title: Prefer List Comprehensions Over Loops
impact: CRITICAL
impactDescription: Clearer and often faster list construction
tags: [performance, loops, comprehension]
---

# Prefer List Comprehensions Over Loops [CRITICAL]

## Description
Instead of building simple lists with `append` loops, use list comprehensions for concise code that is often faster in CPython. Prefer a regular loop when the transformation needs side effects, complex branching, or step-by-step readability.

## Bad Example
```python
# Verbose for simple list construction
result: list[int] = []
for i in range(1000):
    result.append(i * 2)

# Verbose filtering
filtered: list[str] = []
for item in items:
    if item.startswith("prefix_"):
        filtered.append(item.upper())
```

## Good Example
```python
# Concise list construction
result: list[int] = [i * 2 for i in range(1000)]

# Comprehension with filtering
filtered: list[str] = [
    item.upper() for item in items if item.startswith("prefix_")
]
```

## Notes
- If a comprehension exceeds 3 lines, consider using a regular loop for readability
- Use regular loops when side effects are needed (print, file writes, etc.)
- Dict comprehensions `{k: v for k, v in items}` and set comprehensions `{x for x in items}` follow the same pattern
- Benchmark hot paths in the target runtime before relying on a specific speedup

## References
- [Python Docs - List Comprehensions](https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions)
