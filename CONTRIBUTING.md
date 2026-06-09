# Contributing

Thank you for your interest in contributing to python-best-practices.

## Ways to Contribute

- **Add new rules** - Expand coverage with additional best practices
- **Improve existing rules** - Enhance examples, fix errors, or add clarity
- **Report issues** - Found a problem? Open an issue

## Project Structure

```
skills/
├── coding-standards/          # Python coding best practices
│   ├── SKILL.md               # Skill definition and overview
│   ├── metadata.json          # Skill metadata
│   └── rules/
│       ├── _template.md       # Rule template
│       └── {prefix}-{name}.md # Individual rules
└── tooling/                   # Development tool configuration
    ├── SKILL.md
    ├── metadata.json
    └── rules/
```

## Adding a New Rule

### 1. Choose the right skill and category

| Skill | Categories |
|-------|-----------|
| coding-standards | `perf-`, `async-`, `design-`, `oop-` |
| tooling | `analysis-`, `lint-`, `type-`, `fmt-`, `test-`, `pkg-` |

### 2. Create the rule file

Copy the template and create your rule:

```bash
cp skills/{skill}/rules/_template.md skills/{skill}/rules/{prefix}-{name}.md
```

### 3. Write the rule

Follow this structure:

```markdown
---
title: Rule Title
impact: CRITICAL | HIGH | MEDIUM | LOW
impactDescription: Brief impact
tags: [tag1, tag2, tag3]
---

# Rule Title [IMPACT]

## Description
Why this rule matters and when to apply it.

## Bad Example
```python
# Code to avoid
```

## Good Example
```python
# Recommended approach
```

## Notes
- Edge cases and exceptions
- Additional tips

## References
- [Link](URL)
```

### 4. Update SKILL.md

Add your rule to the appropriate category table in `skills/{skill}/SKILL.md`.

### 5. Update metadata.json

Increment the `rules` count in `skills/{skill}/metadata.json`.

## Guidelines

- **Be concise** - Rules should be scannable by AI agents
- **Show, don't tell** - Good/bad examples are more valuable than lengthy explanations
- **Include impact** - Prefer concrete but defensible impact descriptions; avoid fragile benchmark claims without references
- **Add references** - Link to official docs or authoritative sources

## Validation

Run the repository checks before opening a pull request:

```bash
python scripts/validate.py
python -m json.tool skills/coding-standards/metadata.json
python -m json.tool skills/tooling/metadata.json
git diff --check
```

## New Rule Checklist

- File was created from `skills/{skill}/rules/_template.md`
- Rule is listed in the matching category table in `skills/{skill}/SKILL.md`
- `metadata.json.rules` and section counts are updated
- Rule includes `Description`, `Bad Example`, `Good Example`, `Notes`, and `References`
- References point to official docs or authoritative sources
- `scripts/validate.py`, `python -m json.tool`, and `git diff --check` pass

## Pull Requests

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

Keep PRs focused on a single change when possible.
