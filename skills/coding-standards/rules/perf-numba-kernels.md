---
title: Use Numba for Hot Numerical Kernels
impact: CRITICAL
impactDescription: Compiled loops for performance-critical numerical code
tags: [performance, numba, jit, numerical, kernels]
---

# Use Numba for Hot Numerical Kernels [CRITICAL]

## Description
Use Numba for hot numerical kernels when profiling shows plain Python loops are too slow or vectorization would be unreadable or memory-heavy. In Numba kernels, explicit loops are often the clearest style.

## Bad Example
```python
from numba import njit


@njit(cache=True)
def bad_kernel(model, encoded_sequence, logger):
    logger.info("scanning")
    return recognize(model, encoded_sequence)
```

## Good Example
```python
import numpy as np
from numba import njit


@njit(cache=True)
def score_sequence(encoded_seq: np.ndarray, pwm: np.ndarray) -> np.ndarray:
    n = encoded_seq.shape[0]
    k = pwm.shape[1]
    out = np.empty(n - k + 1, dtype=np.float64)

    for i in range(n - k + 1):
        score = 0.0
        for j in range(k):
            base = encoded_seq[i + j]
            score += pwm[base, j]
        out[i] = score

    return out
```

## Notes
- Prefer `@njit(cache=True)` for stable kernels.
- Keep kernels small and pass NumPy arrays plus scalar numeric types.
- Avoid Python objects, dictionaries, pandas objects, file I/O, logging, classes, dynamic dispatch, `toolz`, and `singledispatch` inside kernels.
- Use `parallel=True` only for independent loops where parallel scheduling is worthwhile.
- Use `fastmath=True` only when small numerical differences are acceptable.
- Keep validation, dispatch, I/O, and result packaging in Python orchestration outside the kernel.

## References
- [Numba Docs - Compiling Python code with jit](https://numba.readthedocs.io/en/stable/user/jit.html)
- [Numba Docs - Performance Tips](https://numba.readthedocs.io/en/stable/user/performance-tips.html)
