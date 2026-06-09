---
title: Avoid Functional Overabstraction
impact: MEDIUM
impactDescription: Keeps Python code readable and idiomatic
tags: [design, functional, abstraction, yagni, readability]
---

# Avoid Functional Overabstraction [MEDIUM]

## Description
Use functional style to make data flow clearer, not to recreate a functional language inside Python. Extra containers, combinators, and point-free abstractions should earn their place in the codebase.

## Bad Example
```python
class Maybe:
    def __init__(self, value: object | None) -> None:
        self.value = value

    def map(self, func):
        if self.value is None:
            return Maybe(None)
        return Maybe(func(self.value))


result = Maybe(load_scores(path)).map(normalize_scores).map(summarize_scores)
```

## Good Example
```python
from dataclasses import dataclass

import numpy as np


@dataclass(frozen=True)
class ScoreSummary:
    n: int
    mean: float


def summarize_scores(scores: np.ndarray) -> ScoreSummary:
    return ScoreSummary(n=int(scores.size), mean=float(scores.mean()))
```

## Notes
- Do not introduce monads, `Result`, `Maybe`, `IO`, or custom functional containers unless the project already uses them.
- Use exceptions at I/O and validation boundaries.
- Use `T | None` when absence is a normal expected result.
- Use frozen dataclasses for structured results.
- Use tuples only for short, obvious return values.
- Do not build a mini functional framework for one workflow.

## References
- [Python Docs - Errors and Exceptions](https://docs.python.org/3/tutorial/errors.html)
