# Code Review Skill — General

This directory contains examples and documentation for the `/code-review` prompt skill.

## Skill File

**Prompt file**: [`.github/prompts/code-review.prompt.md`](../../../.github/prompts/code-review.prompt.md)

## How to Use

1. Open any source code file in VS Code.
2. (Optional) Select specific lines to focus the review.
3. Open Copilot Chat (`Ctrl+Alt+I` / `Cmd+Option+I`).
4. Type `/code-review` and press Enter.

## What It Reviews

| Category | Details |
|---|---|
| **Correctness** | Logic errors, off-by-one, unhandled edge cases |
| **Readability** | Naming, structure, unnecessary complexity |
| **Security** | Input validation, injection risks, sensitive data exposure |
| **Performance** | Bottlenecks, inefficient algorithms |
| **Testability** | Function size, side effects, test coverage |
| **Standards** | Docstrings, coding standards compliance |

## Example Output

```
## Code Review Results

### Findings

| # | Severity | Location | Issue | Recommendation |
|---|----------|----------|-------|----------------|
| 1 | Critical | line 42 | Missing input validation on `user_id` | Add: `if not isinstance(user_id, int) or user_id <= 0: raise ValueError(...)` |
| 2 | Major | line 67 | SQL query uses string concatenation | Use parameterized queries to prevent SQL injection |
| 3 | Minor | line 15 | Function `process_data` has no docstring | Add docstring describing parameters and return value |
| 4 | Suggestion | line 30 | Variable name `d` is non-descriptive | Rename to `departure_time` |

### Summary
The function has two significant issues (a missing input validation and a SQL injection risk)
that must be addressed before merging. The minor issues should be fixed in the same PR.

### Verdict: Request Changes
```

## Example Code (Before Review)

```python
def get_user_orders(user_id):
    d = db.connect()
    query = "SELECT * FROM orders WHERE user_id = " + str(user_id)
    result = d.execute(query)
    return result
```

## Example Code (After Review + Fixes)

```python
def get_user_orders(user_id: int) -> list[dict]:
    """
    Retrieve all orders for a given user.

    Args:
        user_id: The unique identifier of the user. Must be a positive integer.

    Returns:
        A list of order dictionaries.

    Raises:
        ValueError: If user_id is not a positive integer.
    """
    if not isinstance(user_id, int) or user_id <= 0:
        raise ValueError(f"user_id must be a positive integer, got {user_id!r}")

    with db.connect() as conn:
        result = conn.execute(
            "SELECT * FROM orders WHERE user_id = ?",
            (user_id,)
        )
        return result.fetchall()
```

## Customization Tips

To focus the review on specific areas, modify the prompt by mentioning your concern:
- `/code-review` then add: "Focus especially on security vulnerabilities."
- `/code-review` then add: "This is Python 3.12 code using FastAPI."
