# GitHub Copilot Custom Instructions — Hitachi Rail GTS

These instructions are automatically injected into every GitHub Copilot Chat conversation
when working in this repository or any repository that includes this file.

---

## Organization Context

You are assisting software engineers at **Hitachi Rail GTS** (Global Technology Solutions).
Our work focuses on railway signalling, control systems, and transportation management software.
Many of our systems are **safety-critical** and must comply with standards such as:

- **EN 50128 / IEC 62279** — Railway software safety integrity levels (SIL 0–4)
- **EN 50657** — Applications of railway equipment
- **CENELEC standards** for railway control and protection systems

---

## General Coding Guidelines

- Prefer **clarity and explicitness** over cleverness. Code will be reviewed by engineers who may not be familiar with advanced language idioms.
- Always add **function/method docstrings** describing purpose, parameters, return values, and any exceptions.
- Include **defensive programming** patterns: validate inputs, handle edge cases, and fail safely.
- Write **deterministic, testable** functions — avoid hidden state and side effects where possible.
- Flag any code that touches **safety-critical paths** with a `# SAFETY-CRITICAL:` comment explaining the concern.

---

## Safety-Critical Code Rules

When generating or reviewing safety-critical code:

1. **No undefined behavior** — All variables must be initialized before use.
2. **Bounded loops** — Every loop must have a clearly defined termination condition.
3. **No dynamic memory allocation** in real-time safety-critical sections (prefer stack allocation or pre-allocated pools).
4. **Error return codes must be checked** — Never ignore return values from functions that can fail.
5. **Numeric overflow protection** — Use range checks before arithmetic operations on safety-critical values.
6. **Thread safety** — Clearly document and protect any shared state with appropriate synchronization primitives.

---

## Documentation Standards

- Use **Markdown** for all documentation files.
- Requirements must be written in the form: `The system SHALL / SHOULD / MAY ...`
- Each function or module should reference the **requirement ID** it implements (e.g., `# Implements: REQ-SIG-042`).

---

## Testing Standards

- Every public function must have at least one **unit test**.
- Test names must follow the pattern: `test_<function_name>_<scenario>_<expected_result>`.
- Include **boundary value** and **equivalence class** test cases.
- Safety-critical modules require **100% branch coverage**.

---

## Language-Specific Notes

### Python
- Follow PEP 8 style.
- Use type hints on all function signatures.
- Prefer `pathlib.Path` over `os.path`.

### C / C++
- Follow MISRA C:2012 guidelines for safety-critical code.
- Use `const` wherever possible.
- Avoid raw pointers; prefer smart pointers in C++.

### Java
- Follow Google Java Style Guide.
- Use `@Nullable` / `@NonNull` annotations where appropriate.

---

## Prompt Engineering Tips

When asking Copilot for help in this repository:

- **Be specific** about the safety integrity level (SIL) of the code you are working on.
- **Reference requirement IDs** when asking Copilot to implement functionality.
- **Ask for test cases** explicitly when generating new functions.
- Use the prompt files in `.github/prompts/` for structured workflows (see README for the list).
