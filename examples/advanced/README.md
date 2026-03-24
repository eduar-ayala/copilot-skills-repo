# Advanced Examples — GitHub Copilot Skills

This directory contains advanced usage patterns for Copilot skills, including multi-step workflows, agent mode, and domain-specific Hitachi Rail GTS scenarios.

---

## Example 1 — Full Safety Review Workflow

This example demonstrates a complete safety review workflow for a new ATP (Automatic Train Protection) module.

### Scenario
You've written a new speed enforcement function and need to:
1. Perform a safety code review
2. Generate SIL-compliant tests
3. Generate traceability documentation

### Step 1 — Safety Code Review

Open `speed_enforcer.py` and in Copilot Chat:
```
/safety-review
This is SIL 3 code implementing REQ-ATP-105.
```

### Step 2 — Fix Issues Found

Address each finding from the review. Example fix:
```
Fix the issues identified in the safety review.
Ensure all inputs are validated and NaN/Inf are handled.
```

### Step 3 — Generate SIL-3 Tests

```
/test-generation
This is SIL 3 code. Target 100% branch coverage.
Include fault injection tests for all sensor failure modes.
```

### Step 4 — Generate Traceability Table

```
Generate a traceability matrix in Markdown table format
linking each function in this file to its requirement ID,
test case names, and SIL level.
#file
```

---

## Example 2 — Requirements to Implementation Pipeline

This example shows how to go from a user story to implemented, tested, documented code.

### Step 1 — Analyze Requirements

Paste a user story into Copilot Chat and run:
```
/requirements-analysis
```

Copy the generated `REQ-XXX-NNN` requirement IDs.

### Step 2 — Generate Code Skeleton

```
Generate a Python module skeleton implementing these requirements:
REQ-ATP-101: Visual alert when speed > limit + 5 km/h
REQ-ATP-102: Audible alert when speed > limit + 5 km/h
REQ-ATP-105: Emergency brake if speed > limit + 15 km/h and not acknowledged within 5s

Include:
- Type-annotated function signatures
- Docstrings referencing the requirement IDs
- SAFETY-CRITICAL comments where appropriate
- Placeholder implementations (raise NotImplementedError)
```

### Step 3 — Implement the Functions

Use Copilot Chat inline (`Ctrl+I`) to implement each function:
```
Implement this function following EN 50128 SIL 3 guidelines.
Validate all inputs. Handle sensor faults by returning a safe state.
```

### Step 4 — Review, Test, Document

Run in sequence:
1. `/safety-review` — Fix any findings
2. `/test-generation` — Generate SIL-3 test suite
3. `/documentation` — Generate module documentation

---

## Example 3 — Using Agent Mode for Multi-File Changes

For a prompt file with `mode: agent`, Copilot can make changes across multiple files.

### Create a custom agent prompt

Create `.github/prompts/refactor-safety-module.prompt.md`:

```yaml
---
mode: agent
description: Refactor a module to comply with EN 50128 SIL 3 requirements
tools:
  - codebase
  - editFiles
---

# Safety Module Refactor

Analyze the attached module and refactor it to comply with EN 50128 SIL 3:

1. Add input validation to all public functions
2. Add NaN/Inf checks for all float inputs
3. Ensure all return values from called functions are checked
4. Add `# SAFETY-CRITICAL:` comments to safety-relevant lines
5. Update docstrings to include SIL level and requirement references
6. Create or update the corresponding test file with 100% branch coverage

#file
```

### Run it

```
/refactor-safety-module
```

Copilot will analyze the code, propose changes across multiple files, and apply them after your approval.

---

## Example 4 — Creating a Custom Skill

Here's how to create a new skill tailored to Hitachi Rail GTS needs.

### Scenario: Changelog Entry Generator

You need a skill to generate release note entries from commit messages.

### Create the prompt file

`.github/prompts/hitachi-rail-gts/changelog-entry.prompt.md`:

```markdown
---
mode: ask
description: Generate a release note entry for Hitachi Rail GTS JIRA/changelog from selected code changes
---

# Changelog Entry Generator

Generate a changelog entry for the code changes shown below.

Format:
- **Type**: [ADDED / CHANGED / FIXED / REMOVED / SECURITY]
- **Component**: The affected system component (e.g., ATP, ETCS, TMS)
- **Summary**: One sentence describing the change (past tense verb, non-technical)
- **Impact**: Who is affected (e.g., Train drivers, Signallers, System administrators)
- **Requirements**: List of requirement IDs addressed (if identifiable from comments)
- **Safety**: YES/NO — Does this change affect safety-critical code?

Example output:
> **[FIXED] ATP Speed Monitor** — Corrected speed threshold comparison to prevent false
> overspeed alerts in GPS-degraded zones. Affects train operators. REQ-ATP-042.
> Safety: YES

#selection
```

### Test it

Select a diff or a changed function, then type `/changelog-entry` in Copilot Chat.

---

## Tips for Advanced Usage

1. **Chain prompts**: Run `/safety-review` first, then `/test-generation` in the same chat thread — Copilot will use the review findings as context for the tests.

2. **Provide SIL context explicitly**: Always state the SIL level when working with safety code. Copilot will adjust its recommendations accordingly.

3. **Iterate with inline chat**: Use `Ctrl+I` for quick targeted fixes, and the side panel for structured analysis workflows.

4. **Save effective prompts as skills**: When you write a prompt in chat that produces great results, formalize it as a `.prompt.md` file and share it via this repository.
