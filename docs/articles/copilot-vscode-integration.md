# GitHub Copilot + Visual Studio Code Integration

> **Reference Article** — Last reviewed: March 2025

This article provides a comprehensive overview of how GitHub Copilot integrates with Visual Studio Code, covering all extension points, configuration options, and how to get the most out of Copilot in your daily development workflow.

---

## Prerequisites

| Requirement | Minimum Version |
|---|---|
| Visual Studio Code | 1.87 |
| GitHub Copilot extension | Latest |
| GitHub Copilot Chat extension | Latest |
| GitHub account with Copilot subscription | Individual, Business, or Enterprise |

---

## Installing GitHub Copilot in VS Code

1. Open VS Code.
2. Go to the Extensions view (`Ctrl+Shift+X` / `Cmd+Shift+X`).
3. Search for **"GitHub Copilot"** and install both:
   - **GitHub Copilot** (inline completions)
   - **GitHub Copilot Chat** (chat interface and slash commands)
4. Sign in with your GitHub account when prompted.
5. Verify the Copilot icon appears in the status bar (bottom of the VS Code window).

---

## Core Features

### 1. Inline Code Completions

Copilot suggests code as you type, shown as grey "ghost text":

- **Accept suggestion**: `Tab`
- **Dismiss suggestion**: `Escape`
- **See next suggestion**: `Alt+]` / `Option+]`
- **See previous suggestion**: `Alt+[` / `Option+[`
- **Open all suggestions panel**: `Ctrl+Enter` / `Cmd+Enter`

### 2. Copilot Chat (Sidebar)

Open the chat panel with `Ctrl+Alt+I` / `Cmd+Option+I` or click the chat icon.

**Built-in chat participants:**
| Participant | Purpose |
|---|---|
| `@workspace` | Ask questions about your entire codebase |
| `@vscode` | Ask about VS Code features and settings |
| `@terminal` | Ask about terminal commands |
| `@github` | Search GitHub issues, PRs, and docs |

**Built-in slash commands:**
| Command | Purpose |
|---|---|
| `/explain` | Explain selected code |
| `/fix` | Fix a bug or issue in selected code |
| `/tests` | Generate tests for selected code |
| `/doc` | Add documentation comments |
| `/new` | Create a new file or project scaffold |
| `/newNotebook` | Create a new Jupyter notebook |

### 3. Inline Chat

Press `Ctrl+I` / `Cmd+I` while code is selected or at the cursor position to open an inline chat panel directly in the editor. Useful for quick edits without switching context.

### 4. Smart Actions

Right-click on selected code → **Copilot** submenu to access:
- Explain this
- Fix this
- Generate tests
- Generate documentation

---

## Customization & Configuration

### VS Code Settings

Access via `Ctrl+,` / `Cmd+,` → search "copilot":

| Setting | Description | Default |
|---|---|---|
| `github.copilot.enable` | Enable/disable Copilot per language | `{ "*": true }` |
| `github.copilot.chat.codeGeneration.useInstructionFiles` | Auto-load `.github/copilot-instructions.md` | `true` |
| `github.copilot.chat.codeGeneration.instructions` | Additional inline instructions | `[]` |
| `github.copilot.chat.localeOverride` | Force response language | `auto` |
| `github.copilot.renameSuggestions.triggerAutomatically` | Auto-suggest renames | `true` |

### Disabling Copilot for Specific File Types

In `settings.json`:
```json
{
  "github.copilot.enable": {
    "*": true,
    "plaintext": false,
    "markdown": false,
    "scminput": false
  }
}
```

### Custom Instructions via Settings

You can provide additional instructions directly in `settings.json` (supplementing the `.github/copilot-instructions.md` file):

```json
{
  "github.copilot.chat.codeGeneration.instructions": [
    {
      "text": "Always use TypeScript strict mode. Prefer functional patterns."
    },
    {
      "file": ".github/team-standards.md"
    }
  ]
}
```

---

## Repository-Level Customization

See [sharing-copilot-skills-in-organization.md](sharing-copilot-skills-in-organization.md) for full details on:

- `.github/copilot-instructions.md` — Auto-loaded custom instructions
- `.github/prompts/*.prompt.md` — Reusable slash commands
- `.vscode/settings.json` — Workspace-level settings

---

## Multi-Root Workspaces

To load Copilot skills from multiple repositories simultaneously:

1. In VS Code, go to **File → Add Folder to Workspace...** and add the `copilot-skills-repo` folder.
2. Save the workspace as a `.code-workspace` file.
3. VS Code will discover `copilot-instructions.md` and `.prompt.md` files from **all** workspace folders.

Example `.code-workspace` file:
```json
{
  "folders": [
    { "path": "../my-railway-project" },
    { "path": "../copilot-skills-repo" }
  ],
  "settings": {
    "github.copilot.chat.codeGeneration.useInstructionFiles": true
  }
}
```

---

## Keyboard Shortcuts Reference

| Action | Windows/Linux | macOS |
|---|---|---|
| Open Copilot Chat | `Ctrl+Alt+I` | `Cmd+Option+I` |
| Inline Chat | `Ctrl+I` | `Cmd+I` |
| Accept suggestion | `Tab` | `Tab` |
| Dismiss suggestion | `Escape` | `Escape` |
| Next suggestion | `Alt+]` | `Option+]` |
| Previous suggestion | `Alt+[` | `Option+[` |
| Open all suggestions | `Ctrl+Enter` | `Cmd+Enter` |
| Toggle Copilot | Status bar icon click | Status bar icon click |

---

## Troubleshooting

| Problem | Solution |
|---|---|
| Copilot not suggesting | Check status bar — may be rate-limited or network issue |
| `copilot-instructions.md` not loaded | Verify `useInstructionFiles` is `true`; reload VS Code window |
| Prompt files not appearing | Ensure files are in `.github/prompts/` with `.prompt.md` extension |
| Sign-in issues | Run `GitHub Copilot: Sign In` from the Command Palette |
| Slow completions | Check network connectivity; corporate proxy may need Copilot allowlisted |

---

## References

- [VS Code Copilot overview](https://code.visualstudio.com/docs/copilot/overview)
- [VS Code Copilot customization](https://code.visualstudio.com/docs/copilot/copilot-customization)
- [GitHub Copilot documentation](https://docs.github.com/en/copilot)
- [VS Code Copilot keyboard shortcuts](https://code.visualstudio.com/docs/copilot/copilot-vscode-features)
