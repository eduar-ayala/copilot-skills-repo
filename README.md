# GitHub Copilot Skills Repository

> **Hitachi Rail GTS — Shared Copilot Skills for the Organization**

This repository is the central hub for sharing GitHub Copilot skills, custom instructions, and prompt files across the Hitachi Rail GTS organization. It connects directly with GitHub Copilot in Visual Studio Code so that any team member can immediately benefit from the curated skills defined here.

---

## 📋 Table of Contents

1. [Purpose](#purpose)
2. [How Copilot Skills Work in VS Code](#how-copilot-skills-work-in-vs-code)
3. [Repository Structure](#repository-structure)
4. [Quick Start — Connect this Repo to Your VS Code](#quick-start--connect-this-repo-to-your-vs-code)
5. [Available Skills & Prompts](#available-skills--prompts)
6. [Reference Articles](#reference-articles)
7. [Contributing](#contributing)

---

## Purpose

The goal of this repository is to:

- **Centralize** Copilot custom instructions and reusable prompt files for the entire Hitachi Rail GTS organization.
- **Accelerate** developer productivity by providing ready-to-use AI-assisted workflows tailored to railway software engineering.
- **Standardize** code quality, documentation, and safety reviews through shared Copilot prompts.
- **Educate** team members on how to create, share, and use Copilot skills effectively.

---

## How Copilot Skills Work in VS Code

GitHub Copilot in VS Code supports several extensibility mechanisms:

| Mechanism | File Location | Scope |
|---|---|---|
| **Custom Instructions** | `.github/copilot-instructions.md` | Repository-wide context injected into every Copilot Chat request |
| **Prompt Files** | `.github/prompts/*.prompt.md` | Reusable slash-command-style prompts invokable from Copilot Chat |
| **VS Code Settings** | `.vscode/settings.json` | Per-workspace Copilot configuration |
| **Copilot Extensions** | GitHub Marketplace | Org-wide chat participants (`@extension-name`) |

When any team member opens this repository (or a repository that references it), VS Code automatically picks up the `copilot-instructions.md` and all `.prompt.md` files, making the skills available in Copilot Chat.

**Key requirement:** Ensure `github.copilot.chat.codeGeneration.useInstructionFiles` is set to `true` in VS Code settings (it is `true` by default in recent versions).

---

## Repository Structure

```
copilot-skills-repo/
│
├── .github/
│   ├── copilot-instructions.md          ← Global custom instructions (auto-loaded by Copilot)
│   └── prompts/                         ← Reusable prompt files (use via Copilot Chat)
│       ├── code-review.prompt.md
│       ├── documentation.prompt.md
│       ├── testing.prompt.md
│       └── hitachi-rail-gts/            ← Domain-specific prompts
│           ├── safety-review.prompt.md
│           ├── requirements-analysis.prompt.md
│           └── test-generation.prompt.md
│
├── docs/
│   ├── articles/                        ← Curated external articles & references
│   │   ├── sharing-copilot-skills-in-organization.md
│   │   ├── copilot-vscode-integration.md
│   │   └── copilot-extensions-overview.md
│   ├── guides/                          ← Step-by-step how-to guides
│   │   ├── getting-started.md
│   │   ├── creating-custom-instructions.md
│   │   ├── creating-prompt-files.md
│   │   └── connecting-vscode.md
│   └── manuals/                         ← Reference manuals
│       ├── copilot-chat-commands.md
│       └── prompt-engineering-guide.md
│
├── skills/
│   ├── general/                         ← General-purpose skill examples
│   │   ├── code-review/
│   │   ├── documentation/
│   │   └── testing/
│   └── hitachi-rail-gts/                ← Hitachi Rail GTS domain skills
│       ├── safety-critical-code/
│       ├── requirements-analysis/
│       └── test-generation/
│
└── examples/
    ├── basic/                           ← Beginner examples
    └── advanced/                        ← Advanced usage patterns
```

---

## Quick Start — Connect this Repo to Your VS Code

### Step 1 — Prerequisites

- Visual Studio Code ≥ 1.87
- GitHub Copilot and GitHub Copilot Chat extensions installed and authenticated
- Access to the `eduar-ayala/copilot-skills-repo` repository

### Step 2 — Clone this Repository

```bash
git clone https://github.com/eduar-ayala/copilot-skills-repo.git
cd copilot-skills-repo
code .
```

### Step 3 — Verify VS Code Settings

Open your VS Code settings (`Ctrl+,` / `Cmd+,`) and confirm:

```json
{
  "github.copilot.chat.codeGeneration.useInstructionFiles": true
}
```

### Step 4 — Use Prompt Files in Copilot Chat

1. Open Copilot Chat (`Ctrl+Alt+I` / `Cmd+Option+I`).
2. Type `/` — you will see the available prompts from `.github/prompts/`.
3. Select a prompt (e.g., `/code-review`) and Copilot will execute it against your current context.

### Step 5 — Use Skills in Your Own Repositories

To bring these skills into another project, you can:

**Option A — Git Submodule**
```bash
git submodule add https://github.com/eduar-ayala/copilot-skills-repo.git .copilot-skills
```

**Option B — Copy Files**
Copy the `.github/copilot-instructions.md` and `.github/prompts/` folder into your repository.

**Option C — Reference in VS Code Workspace**
Add the cloned skills repo as a workspace folder alongside your project. Copilot picks up instructions from all workspace folders.

---

## Available Skills & Prompts

### General Skills

| Prompt File | Description | Usage |
|---|---|---|
| `code-review.prompt.md` | Comprehensive code review checklist | `/code-review` in Copilot Chat |
| `documentation.prompt.md` | Generate structured documentation | `/documentation` in Copilot Chat |
| `testing.prompt.md` | Create unit and integration tests | `/testing` in Copilot Chat |

### Hitachi Rail GTS Skills

| Prompt File | Description | Usage |
|---|---|---|
| `hitachi-rail-gts/safety-review.prompt.md` | Safety-critical code review (IEC 62279 / EN 50128) | `/safety-review` in Copilot Chat |
| `hitachi-rail-gts/requirements-analysis.prompt.md` | Analyze and trace requirements | `/requirements-analysis` in Copilot Chat |
| `hitachi-rail-gts/test-generation.prompt.md` | Generate railway domain test cases | `/test-generation` in Copilot Chat |

---

## Reference Articles

The following curated articles are stored in [`docs/articles/`](docs/articles/):

1. **[Sharing Copilot Skills in Your Organization](docs/articles/sharing-copilot-skills-in-organization.md)**
   — How to distribute custom instructions and prompt files across teams using GitHub.

2. **[GitHub Copilot + VS Code Integration](docs/articles/copilot-vscode-integration.md)**
   — Deep dive into how Copilot integrates with VS Code, including all extension points.

3. **[Copilot Extensions Overview](docs/articles/copilot-extensions-overview.md)**
   — How to build and consume GitHub Copilot Extensions for organization-wide chat participants.

Full guides are in [`docs/guides/`](docs/guides/) and reference manuals are in [`docs/manuals/`](docs/manuals/).

---

## Contributing

1. **Add a new skill** — Create a `.prompt.md` file in `.github/prompts/` (general) or `.github/prompts/hitachi-rail-gts/` (domain-specific). Follow the template in [`docs/guides/creating-prompt-files.md`](docs/guides/creating-prompt-files.md).
2. **Improve an existing skill** — Edit the relevant `.prompt.md` file and submit a Pull Request.
3. **Add an article or guide** — Place it in `docs/articles/` or `docs/guides/` and link it from this README.
4. **Report an issue** — Open a GitHub Issue describing the skill you need or the problem you encountered.

---

*Maintained by the Hitachi Rail GTS Engineering Team. For questions, open an issue or contact the repository maintainers.*