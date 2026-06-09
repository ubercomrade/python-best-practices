---
title: Use Immutable Config and Result Objects
impact: HIGH
impactDescription: Clear structured data without accidental reassignment
tags: [design, functional, dataclass, configuration, results]
---

# Use Immutable Config and Result Objects [HIGH]

## Description
Use typed frozen dataclasses for configuration, model metadata, and structured results. They make data flow explicit without forcing behavior-heavy object hierarchies.

## Bad Example
```python
def scan_motif(options: dict[str, object]) -> dict[str, object]:
    threshold = options["threshold"]
    scores = run_scan(options["motif_path"], options["genome_path"])
    return {"scores": scores, "threshold": threshold}
```

## Good Example
```python
from dataclasses import dataclass
from pathlib import Path

import numpy as np


@dataclass(frozen=True)
class ScanConfig:
    motif_path: Path
    genome_path: Path
    threshold: float
    use_reverse_complement: bool = True


@dataclass(frozen=True)
class MotifScanResult:
    motif_id: str
    positions: np.ndarray
    scores: np.ndarray
```

## Notes
- Prefer dataclasses when the structure is known ahead of time.
- Dataclasses are data containers, not a reason to build a class hierarchy.
- `frozen=True` prevents attribute reassignment, but it does not make mutable fields such as `np.ndarray` immutable.
- Use loose dictionaries only when the shape is genuinely dynamic.
- Keep behavior as pure functions when methods would only wrap one transformation.

## References
- [Python Docs - dataclasses](https://docs.python.org/3/library/dataclasses.html)
- [NumPy Docs - ndarray.flags](https://numpy.org/doc/stable/reference/generated/numpy.ndarray.flags.html)
