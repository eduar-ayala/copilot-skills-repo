# Connecting This Repository to VS Code

> **Guide** — Step-by-step instructions to link the Copilot Skills Repository to your VS Code environment

This guide covers all methods for connecting the Hitachi Rail GTS Copilot Skills Repository to your VS Code workflow so that the shared skills are always available when you code.

---

## Prerequisites

- VS Code ≥ 1.87 installed
- GitHub Copilot and GitHub Copilot Chat extensions installed (see [getting-started.md](getting-started.md))
- Git installed and configured
- Access to `https://github.com/eduar-ayala/copilot-skills-repo`

---

## Method 1 — Open Skills Repo as a Workspace Folder (Recommended)

This is the simplest approach. Add the skills repository as an additional folder in whatever workspace you are working in.

### One-Time Setup

```bash
# Clone the skills repository to a stable local location
git clone https://github.com/eduar-ayala/copilot-skills-repo.git ~/copilot-skills-repo
```

### Per-Project Setup

1. Open your project in VS Code: `code /path/to/your/project`
2. Go to **File → Add Folder to Workspace...**
3. Select `~/copilot-skills-repo` (or wherever you cloned it).
4. Save the workspace: **File → Save Workspace As...** → e.g., `my-project.code-workspace`

From now on, open `my-project.code-workspace` to get both your project and the skills loaded together.

### Keeping Skills Up to Date

```bash
cd ~/copilot-skills-repo
git pull origin main
```

---

## Method 2 — Git Submodule in Your Project

Add the skills repository as a git submodule inside your project. This ties the exact version of the skills to your project's git history.

### Setup

```bash
cd /path/to/your/project

# Add as a submodule
git submodule add https://github.com/eduar-ayala/copilot-skills-repo.git .copilot-skills

# Initialize and fetch
git submodule update --init --recursive

# Commit the submodule reference
git commit -m "chore: add Copilot skills as submodule"
```

### Make Skills Discoverable by VS Code

VS Code looks for `.github/prompts/` in workspace folders. Create a symlink so the skills folder is recognized:

**macOS / Linux:**
```bash
mkdir -p .github
ln -s ../.copilot-skills/.github/prompts .github/prompts
ln -s ../.copilot-skills/.github/copilot-instructions.md .github/copilot-instructions.md
```

**Windows (PowerShell as Administrator):**
```powershell
New-Item -ItemType SymbolicLink -Path ".github\prompts" -Target ".copilot-skills\.github\prompts"
New-Item -ItemType SymbolicLink -Path ".github\copilot-instructions.md" -Target ".copilot-skills\.github\copilot-instructions.md"
```

### Updating the Submodule

```bash
cd .copilot-skills
git pull origin main
cd ..
git add .copilot-skills
git commit -m "chore: update Copilot skills submodule"
```

---

## Method 3 — Copy Files Directly

The simplest method for teams that want full control. Copy the relevant files from this repository into your project.

```bash
# From your project root
cp -r /path/to/copilot-skills-repo/.github/prompts .github/prompts
cp /path/to/copilot-skills-repo/.github/copilot-instructions.md .github/copilot-instructions.md
```

**Caveat**: You will need to manually re-copy files when skills are updated. Consider using Method 1 or 2 for automatic updates.

---

## Method 4 — VS Code Settings (Global Instructions)

To apply the Hitachi Rail GTS custom instructions globally to *all* your projects (without any per-project configuration):

1. Open VS Code Settings (`Ctrl+,` / `Cmd+,`).
2. Search for `codeGeneration.instructions`.
3. Click **"Edit in settings.json"**.
4. Add:

```json
{
  "github.copilot.chat.codeGeneration.instructions": [
    {
      "file": "/path/to/copilot-skills-repo/.github/copilot-instructions.md"
    }
  ]
}
```

Replace `/path/to/copilot-skills-repo` with the absolute path where you cloned the repository.

**Note**: Prompt files (`.prompt.md`) cannot be configured this way — they must be in a workspace folder.

---

## Verifying the Connection

After any of the above setups:

### Check Custom Instructions

1. Open Copilot Chat (`Ctrl+Alt+I` / `Cmd+Option+I`).
2. Ask: `What are the coding standards for this project?`
3. Expected: Copilot mentions Hitachi Rail GTS, EN 50128, safety-critical coding rules, etc.

### Check Prompt Files

1. In Copilot Chat, type `/`.
2. Expected: You see prompts like `/code-review`, `/documentation`, `/testing`, `/safety-review`, etc.
3. Select `/code-review` with a file open. Expected: A structured code review is generated.

---

## Troubleshooting

| Problem | Solution |
|---|---|
| Prompt files not showing in `/` list | Verify `.github/prompts/*.prompt.md` files exist in a workspace folder; reload VS Code window |
| Custom instructions not applied | Check `github.copilot.chat.codeGeneration.useInstructionFiles` is `true`; reload window |
| Submodule empty after clone | Run `git submodule update --init --recursive` |
| Symlinks not working on Windows | Ensure Developer Mode is enabled, or run VS Code as Administrator |
| Instructions file too long | Trim to under 1,500 words; token limits may cause truncation |

---

## See Also

- [getting-started.md](getting-started.md) — First-time setup.
- [docs/articles/copilot-vscode-integration.md](../articles/copilot-vscode-integration.md) — Full integration reference.
- [docs/articles/sharing-copilot-skills-in-organization.md](../articles/sharing-copilot-skills-in-organization.md) — Organization-wide distribution patterns.
