---
mode: ask
description: Generate structured documentation for the selected code or file
---

# Documentation Generator

Please generate comprehensive documentation for the code provided. Produce the following sections:

## 1. Module / File Overview
- What is the purpose of this module?
- What problem does it solve?
- What are its main responsibilities?

## 2. Dependencies
- List all external libraries or modules this code depends on.
- Note any version requirements.

## 3. Public API Reference

For each public function, class, or method, document:

```
### <FunctionName>(param1, param2, ...)

**Description:** <What does this function do?>

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| param1 | type | Yes/No | Description |

**Returns:** `<type>` — Description of the return value.

**Raises / Throws:** List any exceptions or error conditions.

**Example:**
```python
# Example usage
result = function_name(arg1, arg2)
```
```

## 4. Usage Examples
- Provide at least one complete, runnable example showing how to use this module.

## 5. Configuration
- Document any environment variables, configuration files, or settings that affect this module's behavior.

## 6. Notes & Caveats
- Thread safety considerations.
- Known limitations.
- Deprecation notices.

---

Generate the documentation in Markdown format suitable for a `README.md` or a dedicated `docs/` page.

#file
