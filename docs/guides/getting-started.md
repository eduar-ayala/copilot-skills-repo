# Getting Started with Copilot Skills

> **Guide** — For Hitachi Rail GTS developers new to GitHub Copilot

This guide walks you through setting up GitHub Copilot, connecting it to this skills repository, and using your first shared skill in VS Code.

---

## Step 1 — Verify Your Copilot Access

1. Go to [github.com/settings/copilot](https://github.com/settings/copilot).
2. Confirm you have an active Copilot subscription (Individual, Business, or Enterprise).
3. If you see "No Copilot subscription", contact your GitHub organization administrator to be assigned a Copilot Business seat.

---

## Step 2 — Install GitHub Copilot in VS Code

1. Open VS Code.
2. Press `Ctrl+Shift+X` (Windows/Linux) or `Cmd+Shift+X` (macOS) to open Extensions.
3. Search for **"GitHub Copilot"** and install it.
4. Search for **"GitHub Copilot Chat"** and install it.
5. Restart VS Code when prompted.
6. Click the Copilot icon in the bottom status bar and sign in with your GitHub account.

✅ **Verification**: The Copilot icon in the status bar should be white/active (not crossed out).

---

## Step 3 — Clone This Repository

```bash
git clone https://github.com/eduar-ayala/copilot-skills-repo.git
cd copilot-skills-repo
```

---

## Step 4 — Set Up a Multi-Root Workspace (Recommended)

To use the skills from this repository while working on any other project:

1. Open VS Code with your primary project:
   ```bash
   code /path/to/your/project
   ```
2. Go to **File → Add Folder to Workspace...** and add the `copilot-skills-repo` folder.
3. Go to **File → Save Workspace As...** and save as `my-project.code-workspace`.
4. Open the `.code-workspace` file to reload with both folders.

VS Code will now load skills from both repositories simultaneously.

**Alternatively**, open just the skills repository to explore its contents:
```bash
code /path/to/copilot-skills-repo
```

---

## Step 5 — Verify Custom Instructions Are Loaded

1. Open Copilot Chat (`Ctrl+Alt+I` / `Cmd+Option+I`).
2. Type: `What coding standards should I follow in this project?`
3. Copilot should mention Hitachi Rail GTS standards, EN 50128 compliance, and other details from `.github/copilot-instructions.md`.

If Copilot does not reflect the custom instructions:
- Open VS Code Settings (`Ctrl+,` / `Cmd+,`).
- Search for `useInstructionFiles`.
- Ensure `github.copilot.chat.codeGeneration.useInstructionFiles` is checked/`true`.
- Run **"Developer: Reload Window"** from the Command Palette.

---

## Step 6 — Use Your First Prompt File

1. Open any code file in VS Code.
2. Select some code (or leave cursor in the file for whole-file context).
3. Open Copilot Chat and type `/` — you should see available prompts.
4. Select `/code-review` and press Enter.
5. Copilot will perform a structured code review using the shared skill.

---

## Step 7 — Try a Domain-Specific Skill

For safety-critical code reviews:

1. Open a file containing safety-critical logic.
2. Select the relevant code.
3. In Copilot Chat, type `/safety-review` (this is the Hitachi Rail GTS safety review prompt).
4. Review the output against EN 50128 criteria.

---

## What's Next?

- Read [creating-custom-instructions.md](creating-custom-instructions.md) to learn how to write your own instructions.
- Read [creating-prompt-files.md](creating-prompt-files.md) to create new reusable skills.
- Explore the [`skills/`](../../skills/) directory for documented examples.
- Explore the [`examples/`](../../examples/) directory for usage walkthroughs.

---

## Getting Help

- Open an issue in this repository for questions or suggestions.
- See [docs/manuals/copilot-chat-commands.md](../manuals/copilot-chat-commands.md) for a full command reference.
