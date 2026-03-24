# Creating Prompt Files for GitHub Copilot

> **Guide** — How to build reusable `.prompt.md` skill files

Prompt files are reusable, slash-command-style AI workflows stored as Markdown in your repository. Once created, they appear in the Copilot Chat command picker and can be executed by any team member with a single command.

---

## What Is a Prompt File?

A prompt file is a Markdown file with:
- **`.prompt.md` extension**
- Stored in **`.github/prompts/`** within any workspace folder
- Optional **YAML front-matter** for metadata
- A **prompt body** in Markdown

When you type `/` in Copilot Chat, VS Code lists all discovered prompt files. Selecting one sends its content to Copilot, optionally with the current file or selection as context.

---

## File Naming

| File Name | Appears In Chat As |
|---|---|
| `.github/prompts/code-review.prompt.md` | `/code-review` |
| `.github/prompts/hitachi-rail-gts/safety-review.prompt.md` | `/safety-review` |
| `.github/prompts/generate-migration.prompt.md` | `/generate-migration` |

Names are derived from the filename (without `.prompt.md`). Subdirectory names are **not** included in the command name — only the filename matters.

---

## Front-Matter Fields

```yaml
---
mode: ask          # ask | edit | agent (default: ask)
description: A short description shown in the command picker
tools:             # (optional) tools available when mode is "agent"
  - codebase
  - terminal
---
```

| Field | Values | Description |
|---|---|---|
| `mode` | `ask`, `edit`, `agent` | How the prompt executes (see below) |
| `description` | string | Short description shown in the command picker |
| `tools` | list | Tools available when using `agent` mode |

### Mode Explained

| Mode | Behavior |
|---|---|
| `ask` | Sends prompt to chat, returns a text response. No automatic edits. |
| `edit` | Applies code edits directly to the file(s) in context. |
| `agent` | Runs in agentic mode: can use tools, create files, run terminal commands. |

---

## Context Variables

Use these variables in the prompt body to include dynamic context:

| Variable | Description |
|---|---|
| `#file` | Attaches the currently open file |
| `#selection` | Attaches the currently selected text |
| `#codebase` | Allows Copilot to search the entire workspace |
| `#editor` | Context from the active editor |
| `${input:variableName}` | Prompts the user for input when the prompt runs |

---

## Minimal Example

```markdown
---
mode: ask
description: Explain the selected code in plain English
---

# Code Explainer

Please explain the following code in plain English, as if explaining to a junior developer.
Cover:
1. What the code does at a high level.
2. How it works step by step.
3. Any potential issues or non-obvious behaviors.

#selection
```

---

## Full Example with Input Variable

```markdown
---
mode: ask
description: Generate a changelog entry for a pull request
---

# Changelog Entry Generator

Generate a changelog entry for the following change.

**Target version:** ${input:version}
**Change type:** ${input:changeType} (choose: Added / Changed / Fixed / Removed / Security)

The entry should:
- Be one to three sentences.
- Be written for a technical audience but understandable to non-developers.
- Start with a verb in past tense (e.g., "Added", "Fixed", "Improved").
- Reference the affected component or module.

#selection
```

---

## Organizing Prompt Files

For larger organizations, organize prompts in subdirectories:

```
.github/prompts/
├── code-review.prompt.md          ← General skills
├── documentation.prompt.md
├── testing.prompt.md
└── hitachi-rail-gts/              ← Domain-specific skills
    ├── safety-review.prompt.md
    ├── requirements-analysis.prompt.md
    └── test-generation.prompt.md
```

All `.prompt.md` files are discovered regardless of subdirectory depth.

---

## Step-by-Step: Creating a New Skill

### Step 1 — Identify the Workflow

Ask yourself:
- What task do I repeat frequently in Copilot Chat?
- What structured prompt do I always copy-paste?

### Step 2 — Create the File

```bash
# In your skills repository
touch .github/prompts/my-new-skill.prompt.md
```

### Step 3 — Add Front-Matter

```yaml
---
mode: ask
description: Brief description of what this skill does
---
```

### Step 4 — Write the Prompt Body

Structure your prompt with:
- A clear title (`# Title`)
- Explicit instructions for what Copilot should produce
- Context variables (`#file`, `#selection`) as appropriate
- Output format specification (e.g., "return as a Markdown table")

### Step 5 — Test the Prompt

1. Reload VS Code (`Ctrl+Shift+P` → "Developer: Reload Window").
2. Open Copilot Chat, type `/`, and find your new skill.
3. Run it against a representative file or selection.
4. Refine the prompt based on the output quality.

### Step 6 — Submit a Pull Request

Open a PR to this repository with your new skill. Include:
- The `.prompt.md` file.
- An update to the README table of available skills.
- A test result screenshot or description.

---

## Prompt Writing Best Practices

1. **Be explicit about output format** — Tell Copilot exactly what structure you want (Markdown table, JSON, numbered list, etc.).
2. **One skill, one purpose** — Keep each prompt focused. Complex multi-step workflows are better as `agent` mode prompts.
3. **Include examples** — A short example of the desired output dramatically improves consistency.
4. **Use numbered steps** — When you want a structured analysis, numbering the steps forces Copilot to be methodical.
5. **Test with diverse inputs** — Run the prompt against several different files to verify it generalizes well.
6. **Version your prompts** — The repository's git history is your version history. Meaningful commit messages help.

---

## See Also

- [creating-custom-instructions.md](creating-custom-instructions.md) — Repository-wide instructions.
- [connecting-vscode.md](connecting-vscode.md) — Full VS Code setup.
- [docs/articles/sharing-copilot-skills-in-organization.md](../articles/sharing-copilot-skills-in-organization.md) — Org-wide distribution.
- [Official docs: Prompt files](https://code.visualstudio.com/docs/copilot/copilot-customization#_prompt-files-experimental)
