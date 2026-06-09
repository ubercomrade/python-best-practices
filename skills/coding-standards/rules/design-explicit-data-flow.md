---
title: Make Data Flow Explicit
impact: HIGH
impactDescription: Fewer hidden dependencies and easier tests
tags: [design, functional, data-flow, globals, testing]
---

# Make Data Flow Explicit [HIGH]

## Description
Functions should make their inputs, outputs, configuration, random seeds, and time dependencies visible in their signatures. Hidden run-specific globals make code harder to test, reuse, parallelize, and reason about.

## Bad Example
```python
THRESHOLD = 0.8


def filter_values(values: np.ndarray) -> np.ndarray:
    return values[values >= THRESHOLD]
```

## Good Example
```python
def filter_by_threshold(values: np.ndarray, threshold: float) -> np.ndarray:
    return values[values >= threshold]
```

## Notes
- Pass thresholds, config objects, random seeds, current time, and feature flags as parameters.
- Global constants are acceptable for true constants, not per-run configuration.
- Avoid functions that depend on module-level state changed by earlier calls.
- If several parameters travel together, use a typed config object instead of unrelated globals.

## References
- [Python Docs - dataclasses](https://docs.python.org/3/library/dataclasses.html)
