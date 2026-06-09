---
title: Use singledispatch for Type-Based Operations
impact: MEDIUM
impactDescription: Stable public operation names without isinstance chains
tags: [design, functional, dispatch, singledispatch, extensibility]
---

# Use singledispatch for Type-Based Operations [MEDIUM]

## Description
Use `functools.singledispatch` when one public operation has several implementations that depend mostly on the type of the first argument. Keep dispatch in orchestration code, not inside numerical kernels.

## Bad Example
```python
def recognize(model: object, encoded_sequence: np.ndarray) -> np.ndarray:
    if isinstance(model, PWMModel):
        return recognize_pwm(model, encoded_sequence)
    if isinstance(model, BaMMModel):
        return recognize_bamm(model, encoded_sequence)
    if isinstance(model, InMoDeModel):
        return recognize_inmode(model, encoded_sequence)
    raise TypeError(f"Unsupported model type: {type(model).__name__}")
```

## Good Example
```python
from functools import singledispatch


@singledispatch
def recognize(model: object, encoded_sequence: np.ndarray) -> np.ndarray:
    raise TypeError(f"Unsupported model type: {type(model).__name__}")


@recognize.register
def _(model: PWMModel, encoded_sequence: np.ndarray) -> np.ndarray:
    return recognize_pwm(model, encoded_sequence)


@recognize.register
def _(model: BaMMModel, encoded_sequence: np.ndarray) -> np.ndarray:
    return recognize_bamm(model, encoded_sequence)
```

## Notes
- Put the dispatch target first, for example `recognize(model, data)`.
- Keep concrete implementations simple: `recognize_pwm`, `recognize_bamm`, `recognize_inmode`.
- Use dispatch before calling NumPy or Numba kernels.
- Do not call `singledispatch` inside `@numba.njit` functions.
- If dispatch depends on several independent dimensions, use an explicit registry keyed by `(model_type, data_type)` instead.

```python
_RECOGNIZERS = {
    (PWMModel, DNASequence): recognize_pwm_dna,
    (BaMMModel, DNASequence): recognize_bamm_dna,
}


def recognize_pair(model: object, data: object) -> np.ndarray:
    try:
        recognizer = _RECOGNIZERS[(type(model), type(data))]
    except KeyError as error:
        raise TypeError("unsupported model/data combination") from error
    return recognizer(model, data)
```

## References
- [Python Docs - functools.singledispatch](https://docs.python.org/3/library/functools.html#functools.singledispatch)
