---
title: Use Simple Functional Pipelines
impact: HIGH
impactDescription: Clear orchestration without clever abstraction
tags: [design, functional, pipeline, toolz, orchestration]
---

# Use Simple Functional Pipelines [HIGH]

## Description
For analysis workflows, keep the shape obvious: load inputs, prepare inputs, run analysis, then write outputs. Middle functions should be pure or mostly pure; boundary functions may perform I/O.

## Bad Example
```python
def main(config: Config) -> None:
    write_outputs(
        summarize_records(
            filter_records(clean_records(load_records(config.path)), config.min_score)
        ),
        config.output_path,
    )
```

## Good Example
```python
from functools import partial

from toolz import pipe


def run_pipeline(records: list[Record], min_score: float) -> Summary:
    return pipe(
        records,
        clean_records,
        partial(filter_records, min_score=min_score),
        summarize_records,
    )


def main(config: Config) -> None:
    records = load_records(config.input_path)
    summary = run_pipeline(records, config.min_score)
    write_outputs(summary, config.output_path)
```

## Notes
- Prefer named functions over long lambdas.
- Use `functools.partial` for simple parameter binding when it improves readability.
- Use `toolz.pipe` when it makes a pipeline clearer than nested calls.
- Use `curry` sparingly; avoid point-free code that hides the computation.
- Do not use `toolz`, `map`/`filter` tricks, or higher-order abstractions inside `@numba.njit` kernels.

## References
- [Python Docs - functools.partial](https://docs.python.org/3/library/functools.html#functools.partial)
- [Toolz Docs - pipe](https://toolz.readthedocs.io/en/latest/api.html#toolz.functoolz.pipe)
