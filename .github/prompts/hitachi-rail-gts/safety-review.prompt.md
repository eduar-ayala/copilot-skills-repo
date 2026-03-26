---
mode: ask
description: Perform a safety-critical code review aligned with EN 50128 / IEC 62279 for railway software
---

# Safety-Critical Code Review — Hitachi Rail GTS

You are reviewing **safety-critical railway software** for Hitachi Rail GTS.
This code must comply with **EN 50128 / IEC 62279** (Railway software safety integrity levels).

Please evaluate the code against all of the following criteria:

---

## 1. Safety Integrity Level (SIL) Assessment

- What is the apparent SIL level of this code (SIL 0–4)?
- Does the implementation rigor match the stated or apparent SIL?
- Are SIL requirements traceable to this code?

## 2. Defensive Programming

- [ ] All inputs are validated before use.
- [ ] All return values from called functions are checked.
- [ ] No function silently swallows errors.
- [ ] Numeric overflow and underflow are protected against.
- [ ] Division by zero is protected against.
- [ ] Array bounds are checked before access.

## 3. Determinism & Real-Time Behavior

- [ ] No unbounded loops or recursion.
- [ ] No dynamic memory allocation in real-time paths (or clearly justified).
- [ ] Execution time is bounded and predictable.
- [ ] No reliance on undefined behavior (especially C/C++).

## 4. Fault Detection & Handling

- [ ] Faults are detected as early as possible.
- [ ] On detecting a fault, the system enters a **safe state** (fail-safe behavior).
- [ ] Fault handling does not introduce new failure modes.
- [ ] Watchdog mechanisms are in place where required.

## 5. Concurrency & Shared State

- [ ] All shared data is protected by appropriate synchronization (mutex, semaphore, etc.).
- [ ] No race conditions or deadlock risks.
- [ ] Interrupt handlers access only re-entrant functions.
- [ ] Priority inversion is addressed (e.g., priority inheritance protocol).

## 6. Traceability

- [ ] Every function references the requirement(s) it implements.
- [ ] Test cases are traceable to requirements.
- [ ] Safety hazards related to this code are identified.

## 7. Code Metrics

Estimate or report the following if possible:
- **Cyclomatic complexity** per function (target ≤ 10 for SIL 2+).
- **Lines of code** per function (target ≤ 60 for SIL 2+).
- Any functions exceeding these thresholds are flagged.

## 8. Documentation & Comments

- [ ] Every public function has a docstring/comment block.
- [ ] Safety-critical sections are labeled with `# SAFETY-CRITICAL:` or equivalent.
- [ ] Assumptions and preconditions are documented.

---

## Review Output Format

Provide your review as:

1. **Safety Summary** — Overall risk assessment and SIL suitability.
2. **Findings Table**:
   | # | Severity | Location | Rule Violated | Description | Recommendation |
   |---|----------|----------|---------------|-------------|----------------|
3. **Pass / Conditional Pass / Fail** verdict with justification.

#file
