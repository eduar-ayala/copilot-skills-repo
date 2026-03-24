---
mode: ask
description: Generate unit and integration tests for the selected code
---

# Test Generation

Please generate a comprehensive test suite for the code provided. Follow these requirements:

## Test Structure

- Use the project's existing test framework (detect from imports or project files).
- Name each test: `test_<function_name>_<scenario>_<expected_result>`.
- Group related tests in a test class or describe block where appropriate.

## Required Test Categories

### 1. Happy Path Tests
- Test the most common, expected usage of each function.
- Use realistic, representative input values.

### 2. Boundary Value Tests
- Test values at the minimum and maximum of allowed ranges.
- Test empty collections, zero values, and single-element collections.

### 3. Equivalence Class Tests
- Divide input space into equivalence classes and test a representative from each class.

### 4. Error / Exception Tests
- Test that functions raise or return appropriate errors for invalid inputs.
- Test behavior when dependencies fail (use mocks/stubs).

### 5. Edge Case Tests
- Null/None/undefined inputs.
- Very large inputs.
- Concurrent access (if the code is used in multi-threaded contexts).

## Additional Requirements

- Each test must have a clear **Arrange / Act / Assert** structure.
- Add a comment above each test explaining what scenario it covers.
- Mock external dependencies (databases, APIs, file system) to keep tests isolated and fast.
- Aim for **100% branch coverage** of all public functions.

---

Generate the full test file, ready to run without modification.

#file
