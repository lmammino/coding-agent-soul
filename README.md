# 🤖 coding-agent-soul

> Does it really have a soul, though? 🤌 💀

An opinionated `AGENTS.md` template born from personal preference, hard-won
experience, and a healthy amount of frustration with AI agents that write
essays instead of code. It might not be for everyone, and that is fine.

**Use it as a starting point, not a gospel.** Tweak it, break it, make it
yours. Your agent, your rules. 🫡

---

## 🤔 What is AGENTS.md?

[`AGENTS.md`](https://agents.md/) is a markdown file that gives AI coding agents
the context and rules they need to work in your codebase. Supported agents load
the applicable file according to their scope rules. Other tools need a small
compatibility file.

This repo maintains an opinionated, project-agnostic starting point. Copy it
into a project. Then replace the placeholders with project-specific details.

## 🚀 How to use it

### The fun way (let your agent do the work) 🪄

Select a full commit SHA from the
[commit history](https://github.com/lmammino/coding-agent-soul/commits/main).
Do not use `main` when you need a reproducible installation.

Replace `<full-commit-sha>` below. Then paste the prompt into your coding agent:

```text
Use AGENTS.md from this exact revision:
https://raw.githubusercontent.com/lmammino/coding-agent-soul/<full-commit-sha>/AGENTS.md

Before you change anything, inspect all existing agent instruction files.
Preserve their project-specific rules. Do not replace an existing instruction
file without my approval. Show me the proposed diff before any replacement.

Inspect the project before you ask questions. Ask only for information that
you cannot find. Complete every applicable field in the "Project Context"
section. Remove unused headings and all placeholder comments.

Create or update CLAUDE.md as a normal file. Add @AGENTS.md so Claude Code
imports the shared instructions. Preserve any Claude-specific instructions.

Create or update .github/copilot-instructions.md as a normal file. Tell
Copilot to read and follow AGENTS.md at the repository root. Preserve any
Copilot-specific instructions.

Do not create .cursorrules. Cursor reads AGENTS.md directly.
```

The full commit SHA fixes the downloaded content to one repository revision.
The preservation steps protect existing project instructions.

### The manual way (for the hands-on types) 🛠️

1. Select a full commit SHA from the repository history.
2. Download the template to a temporary file:

   ```bash
   curl -fsSL \
     "https://raw.githubusercontent.com/lmammino/coding-agent-soul/<full-commit-sha>/AGENTS.md" \
     -o AGENTS.md.template
   ```

3. Inspect any existing `AGENTS.md` before you copy or merge the template.
4. Fill every applicable field in the "Project Context" section.
5. Remove all unused headings and placeholder comments.
6. Create a normal `CLAUDE.md` file for Claude Code:

   ```text
   @AGENTS.md
   ```

7. Create `.github` when needed.
8. Create a normal `.github/copilot-instructions.md` file for Copilot:

   ```text
   Read and follow AGENTS.md at the repository root before you start work.
   Treat it as the primary repository instruction file.
   ```

9. Do not create `.cursorrules`. Cursor reads `AGENTS.md` directly.

## 📋 What it covers

| Section | What it does |
|---------|-------------|
| 🚧 Boundaries | Safety, approval, scope, and user-change protection |
| 🗣️ Language and Tone | STE-inspired rules, brevity, honesty, and explanation depth |
| 💻 Code Style | Naming, constants, enums, indentation, spacing, comments |
| 🏗️ Architecture | Layered hierarchy, abstraction, member visibility |
| 🔧 Workflow | Assumptions, questions, verification, bug fixing, minimal changes |
| 🧪 Testing | Relevant tests with practical failure reporting |
| 📦 Git Workflow | Commit conventions and external-action approval |
| 🗂️ Project Context | Setup, commands, structure, limits, APIs, data, and security |

## 🛠️ Supported tools

Compatibility last reviewed on 2026-08-25.

| Tool | Instruction file | Notes |
|------|------------------|-------|
| [OpenAI Codex](https://developers.openai.com/codex/guides/agents-md/) | `AGENTS.md` | Codex reads the applicable instruction chain directly. |
| [Claude Code](https://code.claude.com/docs/en/memory) | `CLAUDE.md` | Use `@AGENTS.md` to import the shared rules. This method works on Windows. |
| [Cursor](https://docs.cursor.com/context/rules-for-ai) | `AGENTS.md` | Cursor reads root and nested files. `.cursorrules` is legacy. |
| [GitHub Copilot](https://docs.github.com/en/copilot/reference/custom-instructions-support) | `AGENTS.md` and `.github/copilot-instructions.md` | Support varies by Copilot feature. The compatibility file covers more surfaces. |

Many other tools read `AGENTS.md` directly. Check the
[AGENTS.md tool list](https://agents.md/) for current support.

## ⚖️ License

MIT. Do whatever you want. Just don't blame me if your agent develops a
personality. 🤖✨
