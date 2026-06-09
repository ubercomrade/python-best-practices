---
title: Separate Functional Core from Imperative Shell
impact: HIGH
impactDescription: Easier testing, clearer side-effect boundaries
tags: [design, functional, pure-functions, io-boundaries, testing]
---

# Separate Functional Core from Imperative Shell [HIGH]

## Description
Keep computation in pure or mostly pure functions that accept data and return data. Keep CLI parsing, file I/O, logging, plotting, database access, random seed initialization, and saving results in a thin imperative shell.

## Bad Example
```python
from pathlib import Path

import numpy as np


def normalize_scores(path: Path) -> None:
    scores = np.load(path)
    normalized = (scores - scores.mean()) / scores.std()
    np.save("normalized.npy", normalized)
```

## Good Example
```python
from pathlib import Path

import numpy as np


def normalize_scores(scores: np.ndarray) -> np.ndarray:
    std = scores.std()
    if std == 0:
        return np.zeros_like(scores, dtype=np.float64)
    return (scores - scores.mean()) / std


def main(path: Path, output_path: Path) -> None:
    scores = np.load(path)
    normalized = normalize_scores(scores)
    np.save(output_path, normalized)
```

## Notes
- This rule is the architectural boundary around [design-pure-functions](design-pure-functions.md).
- Core functions should not know about files, stdout, databases, or global runtime state.
- The shell may be impure, but it should mostly coordinate inputs, outputs, logging, and calls into the core.
- Do not mix reading files, computing results, and writing outputs in one large function.

## References
- [Functional Core, Imperative Shell](https://www.destroyallsoftware.com/screencasts/catalog/functional-core-imperative-shell)
