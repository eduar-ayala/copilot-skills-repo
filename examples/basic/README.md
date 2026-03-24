# Basic Examples — GitHub Copilot Skills

This directory contains beginner-friendly examples showing how to use Copilot skills.

---

## Example 1 — Your First Code Review

This example shows how to run the `/code-review` skill on a simple Python function.

### Step 1 — Open the example file

Open `example_function.py` in VS Code (see below).

### Step 2 — Run the skill

1. Open Copilot Chat (`Ctrl+Alt+I`).
2. Type `/code-review` and press Enter.

### Step 3 — Review the output

Copilot will return a structured review with findings and a verdict.

### Example File: `example_function.py`

```python
# example_function.py
# A simple function with some deliberate issues for review practice

def process_user_input(data, user):
    if user == "admin":
        query = "SELECT * FROM users WHERE name = '" + data + "'"
        result = db.execute(query)
        return result
    return []
```

**Expected findings from `/code-review`:**
- `Critical` — SQL injection vulnerability (string concatenation in query)
- `Major` — No input validation on `data` or `user`
- `Minor` — No docstring
- `Suggestion` — Use parameterized queries

---

## Example 2 — Generate Documentation

### Step 1 — Open any Python file
### Step 2 — Type in Copilot Chat:
```
/documentation
```

### Step 3 — Review the generated Markdown

Copilot will generate a complete module documentation page you can copy into your `docs/` folder.

---

## Example 3 — Generate Tests

### Step 1 — Open `example_function.py` (fixed version)

```python
# example_function_fixed.py

def celsius_to_fahrenheit(celsius: float) -> float:
    """
    Convert temperature from Celsius to Fahrenheit.

    Args:
        celsius: Temperature in degrees Celsius.

    Returns:
        Temperature in degrees Fahrenheit.
    """
    return (celsius * 9 / 5) + 32
```

### Step 2 — Type in Copilot Chat:
```
/testing
```

### Step 3 — Review generated tests

Copilot should generate tests including:
- `test_celsius_to_fahrenheit_freezing_point_returns_32`
- `test_celsius_to_fahrenheit_boiling_point_returns_212`
- `test_celsius_to_fahrenheit_absolute_zero_returns_negative_459_67`
- `test_celsius_to_fahrenheit_body_temperature_returns_correct_value`

---

## Next Steps

Once you're comfortable with these basics:
- Try the domain-specific skills in [`examples/advanced/`](../advanced/)
- Read the [prompt engineering guide](../../docs/manuals/prompt-engineering-guide.md) to write your own skills
- Explore the full skill documentation in [`skills/`](../../skills/)
