# Prompt Engineering Guide for GitHub Copilot

> **Manual** — How to write prompts that produce consistently high-quality results

Prompt engineering is the practice of crafting inputs to AI models to get predictable, high-quality outputs. This guide covers techniques specifically relevant to GitHub Copilot and the prompts in this repository.

---

## Core Principles

### 1. Be Specific and Explicit

❌ **Vague**: "Review my code"
✅ **Specific**: "Review this Python function for correctness, security vulnerabilities, and compliance with PEP 8. Return findings as a numbered list with severity (Critical/Major/Minor)."

Copilot cannot read your mind. The more precise your instructions, the more precise the output.

### 2. Specify the Output Format

Always tell Copilot what format you want the response in:

```markdown
Return your response as:
1. A summary paragraph
2. A Markdown table with columns: Issue | Severity | Recommendation
3. A final verdict: Pass / Fail
```

### 3. Provide Examples (Few-Shot Prompting)

Including one or two examples of the desired output dramatically improves consistency:

```markdown
Generate a docstring for this function using the following format:

Example:
"""
Calculate the square root of a number.

Args:
    n (float): The input value. Must be non-negative.

Returns:
    float: The square root of n.

Raises:
    ValueError: If n is negative.
"""
```

### 4. Break Down Complex Tasks

For complex multi-step tasks, break them into numbered steps in your prompt:

```markdown
Analyze this code by:
1. Identifying all inputs and outputs
2. Tracing the execution path for the happy case
3. Identifying all error conditions
4. Evaluating the error handling
5. Summarizing the overall code quality
```

### 5. Use Role Assignment

Give Copilot a role at the start of your prompt to frame its perspective:

```markdown
You are a senior safety engineer reviewing safety-critical railway software.
Your primary concern is EN 50128 compliance and fail-safe behavior.
```

---

## Prompt Anatomy

A well-structured prompt has these parts:

```markdown
[Role Assignment]          ← Who Copilot should be
[Context]                  ← Background information
[Task]                     ← What to do
[Constraints]              ← Rules to follow
[Output Format]            ← How to format the response
[Examples]                 ← (Optional) desired output examples
[Context Variable]         ← #file / #selection
```

### Example

```markdown
You are an expert Python developer and technical writer.

I need documentation for the module below. This is part of a railway signalling system
and must follow our documentation standard.

Please generate:
1. A module-level docstring (one paragraph).
2. A docstring for each public function (Args, Returns, Raises sections).
3. A usage example in a ```python code block.

Follow Google Python Style Guide docstring format.
Do not document private functions (those starting with _).

#file
```

---

## Techniques for Safety-Critical Code

When working with safety-critical railway code, use these prompt strategies:

### Checklist-Based Reviews

Structure safety review prompts as checklists. Copilot will work through each item methodically:

```markdown
Review the following code against this checklist. For each item, answer Yes/No/Partial
and explain your reasoning:

- [ ] All inputs are validated before use
- [ ] All return values from called functions are checked
- [ ] No unbounded loops
- [ ] No undefined behavior
...
```

### SIL-Aware Generation

Specify the SIL level when asking Copilot to generate code:

```markdown
Generate a watchdog timer function for a SIL 3 system.
Requirements:
- Must detect missed heartbeats within 100ms
- Must trigger a safe state (output pin set low) on timeout
- Must be callable from an interrupt context (re-entrant)
- Follow MISRA C:2012 guidelines
```

### Failure Mode Analysis

Ask Copilot to reason about failure modes explicitly:

```markdown
For each function in this module, describe:
1. What happens if the function is called with invalid inputs
2. What happens if the function is interrupted mid-execution
3. What the worst-case failure mode is
4. What safe-state behavior should be triggered
```

---

## Common Pitfalls & How to Avoid Them

| Pitfall | Example | Fix |
|---|---|---|
| Ambiguous scope | "Review the code" | "Review the `process_speed_reading()` function in the attached file" |
| Missing context | "Generate tests" | "Generate pytest unit tests for the `SafetyMonitor` class, targeting 100% branch coverage" |
| Overloaded prompt | Asking 10 things at once | Split into multiple focused prompts |
| No format spec | "What issues did you find?" | "List findings as: Issue | Severity | Line Number | Fix" |
| Assuming knowledge | "Follow our standards" | Paste the actual standards or reference `.github/copilot-instructions.md` |

---

## Iterative Refinement

Don't try to perfect a prompt on the first attempt. Use this cycle:

```
Write prompt → Run it → Evaluate output → Identify the gap → Refine the prompt → Repeat
```

**Evaluation questions:**
- Was the output in the correct format?
- Did Copilot address all the criteria?
- Was anything missing or hallucinated?
- Would a different role assignment produce a better result?

---

## Prompt Templates

### Code Review Template
```markdown
You are a [senior developer / safety engineer / security expert].
Review the following code for [correctness / security / performance / safety].
Return findings as a table: Issue | Severity | Location | Recommendation.
Finish with a Pass / Request Changes verdict.
#file
```

### Test Generation Template
```markdown
Generate [pytest / JUnit / Jest] tests for the following code.
Coverage target: [80% statement / 100% branch / 100% MC/DC].
Include: happy path, boundary values, error cases, and edge cases.
Use Arrange/Act/Assert structure.
Name tests as: test_<function>_<scenario>_<expected>.
#file
```

### Documentation Template
```markdown
Generate [Google / NumPy / JSDoc] style documentation for this module.
Include: module overview, public API reference with examples, configuration, and known limitations.
Return as Markdown suitable for a docs/ page.
#file
```

---

## References

- [OpenAI prompt engineering guide](https://platform.openai.com/docs/guides/prompt-engineering)
- [GitHub Copilot best practices](https://docs.github.com/en/copilot/using-github-copilot/best-practices-for-using-github-copilot)
- [VS Code Copilot tips](https://code.visualstudio.com/docs/copilot/prompt-crafting)
