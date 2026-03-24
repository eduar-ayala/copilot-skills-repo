# Sharing GitHub Copilot Skills Across Your Organization

> **Reference Article** — Last reviewed: March 2025

This article explains how to distribute GitHub Copilot custom instructions and reusable prompt files across your organization so that all developers automatically benefit from the same AI-assisted workflows.

---

## Overview

GitHub Copilot supports two primary extensibility mechanisms that can be shared at the repository and organization level:

1. **Custom Instructions** (`.github/copilot-instructions.md`) — Contextual guidance that is automatically injected into every Copilot Chat request.
2. **Prompt Files** (`.github/prompts/*.prompt.md`) — Reusable, slash-command-style prompts that team members invoke in Copilot Chat.

Both mechanisms work natively with Visual Studio Code and require no additional tooling beyond the GitHub Copilot Chat extension.

---

## Method 1: Repository-Level Instructions

### How It Works

When GitHub Copilot Chat is active in VS Code and the user opens a repository that contains `.github/copilot-instructions.md`, VS Code automatically reads that file and prepends its contents to every chat request sent to Copilot.

### Setting Up

1. In your repository, create the file:
   ```
   .github/copilot-instructions.md
   ```
2. Write your organization context, coding standards, and domain knowledge in plain Markdown.
3. Commit and push. Every developer who clones the repository and uses Copilot Chat will get your instructions automatically.

### Best Practices

- Keep instructions **concise** — very long instruction files may exceed token limits.
- Focus on **context that changes behavior**: domain vocabulary, coding standards, safety rules.
- Avoid duplicating information that is already in the codebase (Copilot reads the files too).
- Version control the instructions like any other code artifact.

**Official documentation:** [Customizing GitHub Copilot Chat responses](https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot)

---

## Method 2: Prompt Files

### How It Works

Prompt files are Markdown files with a `.prompt.md` extension stored in `.github/prompts/`. They appear as slash commands in the Copilot Chat interface (e.g., `/code-review`).

When a developer types the command, Copilot executes the prompt — optionally with the currently selected file or code as context — and returns the result.

### Setting Up

1. Create the directory:
   ```
   .github/prompts/
   ```
2. Add a prompt file, e.g. `.github/prompts/code-review.prompt.md`:
   ```markdown
   ---
   mode: ask
   description: Perform a comprehensive code review
   ---
   # Code Review
   Review the following code for correctness, security, and maintainability...
   #file
   ```
3. The `mode` front-matter field accepts: `ask` (default), `edit`, or `agent`.
4. The `#file` variable at the end attaches the current file as context.

### Sharing Across Multiple Repositories

To share prompts without copying files into every repository, use one of the following patterns:

#### Pattern A — Dedicated Skills Repository (Recommended)
Create a single repository (like this one) containing all `.prompt.md` files.
Developers clone it as a workspace folder alongside their project:

```
my-project/          ← project repository
copilot-skills-repo/ ← this repository (cloned separately)
```

Open both folders in a VS Code Multi-Root Workspace (`.code-workspace` file). Copilot discovers prompt files from all workspace folders.

#### Pattern B — Git Submodule
Add the skills repository as a submodule in every project repository:

```bash
git submodule add https://github.com/<org>/copilot-skills-repo.git .copilot-skills
```

Then symlink or copy the `.github/prompts/` folder:
```bash
ln -s .copilot-skills/.github/prompts .github/prompts
```

#### Pattern C — GitHub Actions Sync
Use a GitHub Actions workflow to automatically copy prompt files from the skills repository into each project repository via a scheduled pull request.

**Official documentation:** [Using prompt files with GitHub Copilot](https://code.visualstudio.com/docs/copilot/copilot-customization#_prompt-files-experimental)

---

## Method 3: Copilot Extensions (Organization-Wide Chat Participants)

For more advanced scenarios, GitHub Copilot Extensions allow you to create a custom `@chat-participant` that is available to all users in your organization.

### Key Features
- Accessible via `@your-extension` in any repository.
- Can call external APIs, query internal knowledge bases, or execute custom logic.
- Publishable privately to your GitHub organization.

### Use Cases for Hitachi Rail GTS
- `@rail-standards` — Query EN 50128 / EN 50657 clause references.
- `@requirements-db` — Search the internal requirements management system.
- `@test-runner` — Trigger CI test runs directly from Copilot Chat.

**Official documentation:** [Building GitHub Copilot Extensions](https://docs.github.com/en/copilot/building-copilot-extensions/about-building-copilot-extensions)

---

## Governance & Maintenance

### Recommended Workflow

1. **Propose** — Open an issue or PR in the skills repository with a proposed new or updated skill.
2. **Review** — At least one team member reviews for accuracy, safety, and usefulness.
3. **Merge** — Skill is merged into `main` and becomes available to all users.
4. **Announce** — Notify the team via the preferred channel (Slack, Teams, email).

### Quality Checklist for New Skills

- [ ] The prompt produces consistent, useful results when tested.
- [ ] The prompt does not request or expose sensitive data.
- [ ] The prompt includes a clear `description` in the front-matter.
- [ ] The skill is documented in the repository README.

---

## References

- [GitHub Copilot documentation](https://docs.github.com/en/copilot)
- [VS Code Copilot customization](https://code.visualstudio.com/docs/copilot/copilot-customization)
- [GitHub Copilot Extensions](https://docs.github.com/en/copilot/building-copilot-extensions)
- [GitHub Blog: Copilot custom instructions](https://github.blog/2025-01-09-supercharge-your-coding-workflow-with-github-copilots-new-customization-features/)
