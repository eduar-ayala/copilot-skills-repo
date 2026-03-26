# GitHub Copilot Chat — Command Reference Manual

> **Manual** — Complete reference for built-in commands, participants, and slash commands

---

## Chat Participants

Chat participants are `@mention`-able entities in Copilot Chat that specialize in different domains. Type `@` in the chat input to see the list.

### Built-In Participants

| Participant | Description | Example Usage |
|---|---|---|
| `@workspace` | Understands your entire codebase across all files | `@workspace Where is the authentication logic?` |
| `@vscode` | Answers questions about VS Code features and settings | `@vscode How do I configure the integrated terminal?` |
| `@terminal` | Helps with shell commands and terminal output | `@terminal Why did my npm build fail?` |
| `@github` | Searches GitHub issues, PRs, commits, and documentation | `@github Find open issues related to authentication` |

### Custom / Extension Participants

If your organization has Copilot Extensions installed, they appear here as `@extension-name`. See [docs/articles/copilot-extensions-overview.md](../articles/copilot-extensions-overview.md).

---

## Built-In Slash Commands

Type `/` in Copilot Chat to see available commands. Built-in commands:

| Command | Description | Context Needed |
|---|---|---|
| `/explain` | Explain selected code or a file | Code selection or open file |
| `/fix` | Suggest a fix for the selected code | Code with a bug/error |
| `/tests` | Generate unit tests for selected code | Code to test |
| `/doc` | Add documentation comments to selected code | Code to document |
| `/new` | Create a new file or project scaffold | Description of what you want |
| `/newNotebook` | Create a Jupyter notebook | Description of the notebook |
| `/clear` | Clear the chat history | — |
| `/help` | Show Copilot help | — |

---

## Prompt File Slash Commands (This Repository)

These commands come from the `.github/prompts/` directory in this repository:

### General Skills

| Command | Description | How to Use |
|---|---|---|
| `/code-review` | Structured code review (correctness, security, performance, testing) | Open a file → `/code-review` |
| `/documentation` | Generate comprehensive module documentation | Open a file → `/documentation` |
| `/testing` | Generate unit + integration tests | Open a file → `/testing` |

### Hitachi Rail GTS Skills

| Command | Description | How to Use |
|---|---|---|
| `/safety-review` | EN 50128 safety-critical code review | Open safety-critical code → `/safety-review` |
| `/requirements-analysis` | Decompose and analyze requirements | Paste requirements text → `/requirements-analysis` |
| `/test-generation` | Generate SIL-aware test cases | Open a module → `/test-generation` |

---

## Context Variables in Prompts

When writing or using prompts, these variables attach context:

| Variable | What It Attaches |
|---|---|
| `#file` | The currently open file |
| `#selection` | The currently selected text |
| `#codebase` | Access to the entire workspace codebase |
| `#editor` | Current editor content and cursor position |
| `#terminalLastCommand` | Output of the last terminal command |
| `#terminalSelection` | Selected text in the terminal |

---

## Keyboard Shortcuts

| Action | Windows/Linux | macOS |
|---|---|---|
| Open Copilot Chat panel | `Ctrl+Alt+I` | `Cmd+Option+I` |
| Open Inline Chat (in editor) | `Ctrl+I` | `Cmd+I` |
| Accept inline suggestion | `Tab` | `Tab` |
| Dismiss inline suggestion | `Escape` | `Escape` |
| Next suggestion | `Alt+]` | `Option+]` |
| Previous suggestion | `Alt+[` | `Option+[` |
| Open all completions panel | `Ctrl+Enter` | `Cmd+Enter` |
| Quick chat (floating) | `Ctrl+Shift+Alt+L` | `Cmd+Shift+Option+L` |

---

## Chat Tips & Tricks

### 1. Attach Files Explicitly
Drag a file from the Explorer into the chat, or click the paperclip icon, to attach it to your question.

### 2. Reference Symbols
In chat, type `#` to reference specific symbols, files, or workspace elements:
- `#MyClass` — Reference a class
- `#my-function` — Reference a function

### 3. Iterate on Responses
After receiving a response, follow up in the same chat thread — Copilot retains context within a session. Example:
```
/code-review
> (Copilot returns findings)
Now fix the Critical issues you found
```

### 4. Use Inline Chat for Quick Edits
For small edits, `Ctrl+I` is faster than the side panel. The result appears as a diff you can accept or reject.

### 5. Ask Copilot to Explain Its Reasoning
Add "Explain your reasoning step by step" to any prompt for more transparent responses.

### 6. Set the Language
If you prefer responses in a specific language, add it to your instructions or ask directly:
```
/code-review
Please respond in Spanish.
```

---

## Rate Limits & Quotas

GitHub Copilot applies rate limits depending on your subscription tier. If you see a "rate limit" message:
- Wait a few minutes and retry.
- For Business/Enterprise plans, contact your GitHub administrator to review quota usage.

---

## Reporting Issues with Skills

If a prompt produces poor or incorrect results:
1. Open an issue in this repository describing the problem.
2. Include the prompt used, the code provided, and the problematic response.
3. Suggest how the prompt should be improved.

---

## References

- [VS Code Copilot features reference](https://code.visualstudio.com/docs/copilot/copilot-vscode-features)
- [GitHub Copilot documentation](https://docs.github.com/en/copilot)
- [Prompt files documentation](https://code.visualstudio.com/docs/copilot/copilot-customization#_prompt-files-experimental)
