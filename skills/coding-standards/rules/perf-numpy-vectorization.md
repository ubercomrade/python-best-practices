---
title: Prefer Readable NumPy Vectorization
impact: CRITICAL
impactDescription: Efficient array operations with explicit shape and memory tradeoffs
tags: [performance, numpy, vectorization, arrays, memory]
---

# Prefer Readable NumPy Vectorization [CRITICAL]

## Description
For array-heavy numerical code, prefer NumPy broadcasting, ufuncs, reductions, boolean indexing, and advanced indexing when they are readable and memory-safe. Profile before replacing clear code with complex vectorization.

## Bad Example
```python
def log_odds_loop(pwm: np.ndarray, background: np.ndarray) -> np.ndarray:
    out = np.empty_like(pwm, dtype=np.float64)
    for base in range(pwm.shape[0]):
        for pos in range(pwm.shape[1]):
            out[base, pos] = np.log2(pwm[base, pos] / background[base])
    return out


# np.vectorize is convenience, not a performance tool.
log_odds = np.vectorize(lambda value, bg: np.log2(value / bg))
```

## Good Example
```python
def log_odds(pwm: np.ndarray, background: np.ndarray) -> np.ndarray:
    if pwm.ndim != 2:
        raise ValueError("pwm must be 2D")
    if background.shape != (pwm.shape[0],):
        raise ValueError("background must match the first pwm axis")

    pwm = np.asarray(pwm, dtype=np.float64)
    background = np.asarray(background, dtype=np.float64)
    return np.log2(pwm / background[:, None])
```

## Notes
- Check shapes explicitly when a broadcasting mistake could silently produce wrong results.
- Choose predictable numeric dtypes and avoid accidental `object` arrays.
- Avoid repeated list-to-array and array-to-list conversions inside hot paths.
- Large vectorized expressions can allocate temporary arrays; watch memory pressure.
- `np.vectorize` is mainly for convenience and does not make Python functions fast.
- Use Numba or a clear loop when vectorization would be unreadable or too memory-heavy.

## References
- [NumPy Docs - Broadcasting](https://numpy.org/doc/stable/user/basics.broadcasting.html)
- [NumPy Docs - Indexing](https://numpy.org/doc/stable/user/basics.indexing.html)
- [NumPy Docs - numpy.vectorize](https://numpy.org/doc/stable/reference/generated/numpy.vectorize.html)
