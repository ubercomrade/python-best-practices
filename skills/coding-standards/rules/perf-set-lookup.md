---
title: Use Set for Fast Lookups
impact: CRITICAL
impactDescription: Faster repeated membership checks
tags: [performance, data-structures, set, lookup]
---

# Use Set for Fast Lookups [CRITICAL]

## Description
The `in` operator on lists scans items sequentially, while sets use hash-based membership checks. For repeated membership checks against the same collection, converting to a set is often faster even after paying the one-time conversion cost.

## Bad Example
```python
# Repeated list search scans the list each time
allowed_users: list[str] = ["alice", "bob", "charlie", ...]  # large list

def is_allowed(user: str) -> bool:
    return user in allowed_users

# Filtering repeats the membership scan
valid_ids: list[int] = [1, 2, 3, 4, 5, ...]  # large list
result = [item for item in items if item.id in valid_ids]
```

## Good Example
```python
# Set membership uses hash-based lookup
allowed_users: set[str] = {"alice", "bob", "charlie", ...}

def is_allowed(user: str) -> bool:
    return user in allowed_users

# Filtering avoids repeated list scans
valid_ids: set[int] = {1, 2, 3, 4, 5, ...}
result = [item for item in items if item.id in valid_ids]

# Converting from list
user_list: list[str] = get_users_from_db()
user_set: set[str] = set(user_list)
```

## Notes
- Set elements must be hashable (immutable)
- If order matters, use `dict.fromkeys()` or `dict` in Python 3.7+
- `frozenset` is immutable and can be used as dict keys or set elements
- Sets also deduplicate, but `list(set(items))` does not preserve order
- Do not convert to a set for a single lookup unless the input is already set-like or large enough to justify it

## References
- [Python Docs - Set Types](https://docs.python.org/3/library/stdtypes.html#set)
- [Python Wiki - Time Complexity](https://wiki.python.org/moin/TimeComplexity)
