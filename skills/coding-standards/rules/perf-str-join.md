---
title: Use join for String Concatenation
impact: CRITICAL
impactDescription: Efficient string building from many parts
tags: [performance, string, join]
---

# Use join for String Concatenation [CRITICAL]

## Description
Repeated string concatenation in loops creates new string objects and can become inefficient as the number of parts grows. Use `str.join()` when building a string from many pieces, especially inside loops or comprehensions.

## Bad Example
```python
# Repeated concatenation creates intermediate strings
def build_csv_line(values: list[str]) -> str:
    result = ""
    for value in values:
        result += value + ","
    return result[:-1]

# String concatenation in loop
log_message = ""
for event in events:
    log_message += f"[{event.time}] {event.message}\n"
```

## Good Example
```python
# join builds from all parts directly
def build_csv_line(values: list[str]) -> str:
    return ",".join(values)

# Combine with generator expression
log_message = "\n".join(
    f"[{event.time}] {event.message}" for event in events
)

# Filtering with conditions
result = ", ".join(name for name in names if name)
```

## Notes
- f-strings are fine for small fixed-size concatenations
- `io.StringIO` is an alternative but `join()` is usually sufficient
- For bytes, use `b"".join()`
- For paths, use `pathlib.Path` with `/` operator: `Path("dir") / "file.txt"`
- If each loop iteration must interleave writes with other work, consider streaming to a file-like object

## References
- [Python Docs - str.join()](https://docs.python.org/3/library/stdtypes.html#str.join)
