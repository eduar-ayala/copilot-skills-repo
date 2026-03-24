---
mode: ask
description: Perform a comprehensive code review of the selected code or current file
---

# Code Review

Please perform a thorough code review of the following code. Evaluate it against these criteria:

## 1. Correctness
- Does the logic produce the expected output for all valid inputs?
- Are there off-by-one errors, incorrect comparisons, or logic inversions?
- Are all error paths and edge cases handled?

## 2. Readability & Maintainability
- Are variable and function names descriptive?
- Is the code well-structured and easy to follow?
- Is there any unnecessary complexity that could be simplified?
- Are comments present where the code is non-obvious?

## 3. Security
- Are inputs validated and sanitized?
- Are there any injection risks (SQL, command, path traversal)?
- Is sensitive data handled securely (not logged, not hard-coded)?
- Are dependencies up to date and free of known vulnerabilities?

## 4. Performance
- Are there any obvious performance bottlenecks (e.g., nested loops on large data sets, missing indexes)?
- Is memory being used efficiently?

## 5. Testability
- Are functions small and focused enough to be unit-tested?
- Are side effects isolated?
- Are there existing tests that cover this code? If not, what tests should be added?

## 6. Standards Compliance
- Does the code follow the project's coding standards?
- Are docstrings / comments present on all public functions?

---

Please provide your review as a structured list of findings, each with:
- **Severity**: Critical / Major / Minor / Suggestion
- **Location**: File and line number (if applicable)
- **Issue**: Description of the problem
- **Recommendation**: How to fix it

Then provide an overall summary and a recommendation (Approve / Request Changes / Needs Discussion).

#file
