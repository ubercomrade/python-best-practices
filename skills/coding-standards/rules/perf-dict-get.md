---
title: Use dict.get() for Efficient Default Values
impact: CRITICAL
impactDescription: Avoid KeyError + cleaner code
tags: [performance, dict, defensive-coding]
---

# Use dict.get() for Efficient Default Values [CRITICAL]

## Description
When retrieving optional keys from dictionaries, `dict.get(key, default)` provides a concise way to specify default values. It avoids a separate membership check and is usually clearer than `try/except` when missing keys are expected.

## Bad Example
```python
# Verbose: existence check + retrieval
config: dict[str, int] = {"timeout": 30}

if "retries" in config:
    retries = config["retries"]
else:
    retries = 3

# Avoid exception control flow when missing keys are expected
try:
    retries = config["retries"]
except KeyError:
    retries = 3
```

## Good Example
```python
config: dict[str, int] = {"timeout": 30}

# Simple: get() with default value
retries = config.get("retries", 3)

# setdefault() sets value if missing and returns it
cache: dict[str, list[int]] = {}
cache.setdefault("results", []).append(42)
```

## Notes
- Omitting the second argument to `get()` returns `None`
- Chain for nested dicts: `dict.get("key1", {}).get("key2", default)`
- Use `setdefault()` when you intentionally want to insert and reuse a default value
- Python 3.8+ allows combining with `:=` (walrus operator)
- If the default is expensive to create, compute it lazily instead of passing it directly to `get()`

## References
- [Python Docs - dict.get()](https://docs.python.org/3/library/stdtypes.html#dict.get)
