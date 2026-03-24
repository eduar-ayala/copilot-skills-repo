# Creating Custom Instructions for GitHub Copilot

> **Guide** — How to write and maintain `.github/copilot-instructions.md`

Custom instructions let you inject persistent context into every GitHub Copilot Chat conversation without repeating yourself. This guide covers how to write effective instructions for your team.

---

## What Are Custom Instructions?

The file `.github/copilot-instructions.md` in your repository root is automatically picked up by VS Code's GitHub Copilot Chat extension. Its contents are prepended to every chat request, giving Copilot relevant context about:

- Your organization and project domain.
- Coding standards and style guides.
- Safety or compliance requirements.
- Technology stack preferences.

---

## File Location & Format

```
your-repository/
└── .github/
    └── copilot-instructions.md   ← plain Markdown, no special syntax required
```

The file is plain **Markdown**. No YAML front-matter is needed.

---

## Structure of Effective Instructions

A well-structured `copilot-instructions.md` should cover:

### 1. Organization Context (2–5 sentences)
Tell Copilot who you are and what you build.

```markdown
## Organization Context
You are assisting engineers at Acme Corp, a company that builds real-time logistics software.
Our systems process millions of shipment events per day and must have 99.99% uptime.
```

### 2. Coding Standards
Reference your style guide and list the most important rules.

```markdown
## Coding Standards
- Follow PEP 8 for Python code.
- Use type hints on all function signatures.
- Maximum function length: 50 lines.
- Prefer explicit over implicit code.
```

### 3. Domain Vocabulary
Define acronyms and domain terms to reduce ambiguity.

```markdown
## Domain Vocabulary
- **SIL**: Safety Integrity Level (EN 50128)
- **ATP**: Automatic Train Protection
- **ETCS**: European Train Control System
```

### 4. Technology Stack
List the frameworks, languages, and tools in use.

```markdown
## Technology Stack
- Language: Python 3.12
- Framework: FastAPI
- Database: PostgreSQL 16 with SQLAlchemy ORM
- Testing: pytest with pytest-asyncio
```

### 5. What NOT to Do
Explicitly telling Copilot what to avoid is as important as what to do.

```markdown
## Avoid
- Do not use `os.system()` or `subprocess.run(shell=True)`.
- Do not hard-code credentials or API keys.
- Do not use deprecated APIs.
```

---

## Length & Token Considerations

- Keep your instructions file under **~1,500 words** (approximately 2,000 tokens).
- Copilot models have context windows; very long instruction files can crowd out your actual question.
- Use **bullet points** and **headings** — they are more token-efficient than paragraphs.
- Focus on instructions that **change behavior**. Don't repeat things Copilot already knows (e.g., don't explain what Python is).

---

## Layering Instructions

You can layer instructions from multiple sources:

| Source | Scope | Priority |
|---|---|---|
| `.github/copilot-instructions.md` | Repository-wide | Applied automatically |
| VS Code settings (`codeGeneration.instructions`) | Workspace/global | Applied on top of file |
| Inline chat context | Per-conversation | Highest (overrides others) |

This means you can have shared org-level instructions in VS Code settings, project-specific instructions in the repository file, and further refinements directly in chat.

---

## Validating Your Instructions

After updating the instructions file:

1. **Reload the VS Code window** (`Ctrl+Shift+P` → "Developer: Reload Window").
2. Open Copilot Chat and ask: `What are the coding standards for this project?`
3. Verify the response reflects your instructions.

---

## Template

Copy the template below into `.github/copilot-instructions.md` and customize it:

```markdown
# Copilot Custom Instructions

## Organization Context
<Who you are and what you build — 2–5 sentences>

## Coding Standards
- <Standard 1>
- <Standard 2>
- <Standard 3>

## Technology Stack
- Language: <language and version>
- Framework: <framework>
- Testing: <test framework>

## Naming Conventions
- <Convention 1>
- <Convention 2>

## Documentation Requirements
- <Documentation standard>

## Security Rules
- <Security rule 1>
- <Security rule 2>

## Avoid
- <Thing to avoid 1>
- <Thing to avoid 2>
```

---

## See Also

- [creating-prompt-files.md](creating-prompt-files.md) — Create reusable slash commands.
- [connecting-vscode.md](connecting-vscode.md) — Full VS Code setup guide.
- [docs/articles/sharing-copilot-skills-in-organization.md](../articles/sharing-copilot-skills-in-organization.md) — Sharing skills across your org.
- [Official docs: Repository custom instructions](https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot)
