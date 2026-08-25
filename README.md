# 🤖 agents.md

> Does it really have a soul, though? 🤌 💀

An opinionated `AGENTS.md` template born from personal preference, hard-won
experience, and a healthy amount of frustration with AI agents that write
essays instead of code. It might not be for everyone — and that is fine.

**Use it as a starting point, not a gospel.** Tweak it, break it, make it
yours. Your agent, your rules. 🫡

---

## 🤔 What is AGENTS.md?

`AGENTS.md` is a markdown file that gives AI coding agents the context and
rules they need to work in your codebase. It is injected into every agent
session, so every instruction matters. A good `AGENTS.md` is the difference
between an agent that writes clean, layered code and one that leaves a trail
of magic numbers and 8-level nested ifs. 😱

This repo maintains a universal, project-agnostic version. You copy it into
a project, fill in the project-specific bits, and go.

## 🚀 How to use it

### The fun way (let your agent do the work) 🪄

Open a chat with your favorite coding agent and paste this:

```text
Pull the AGENTS.md file from this repository:
https://raw.githubusercontent.com/lmammino/coding-agent-soul/main/AGENTS.md

Then help me customize it for this project. Ask me about my tech stack,
build commands, test commands, and project structure. Update the "Project
Context" section with my answers. Create symlinks for CLAUDE.md,
.github/copilot-instructions.md, and .cursorrules so all tools read the
same file.
```

The agent will fetch the file, ask you a few questions, and set everything
up. You know — the thing you were going to do manually anyway, but now it
takes 30 seconds. ⏱️

### The manual way (for the hands-on types) 🛠️

1. Copy `AGENTS.md` to the root of your project.
2. Create symlinks so each tool reads the same file:

   ```bash
   ln -s AGENTS.md CLAUDE.md
   ln -s AGENTS.md .github/copilot-instructions.md
   ln -s AGENTS.md .cursorrules
   ```

3. Fill in the "Project Context" section at the bottom with your tech stack,
   build commands, test commands, and project structure.

## 📋 What it covers

| Section | What it does |
|---------|-------------|
| 🚧 Boundaries | Always / ask first / never — keeps agents from going rogue |
| 🗣️ Language and Tone | STE rules, brevity, honesty, depth of explanation |
| 💻 Code Style | Naming, constants, enums, indentation, spacing, comments |
| 🏗️ Architecture | Layered hierarchy, abstraction, member visibility |
| 🔧 Workflow | Understand before act, verification, bug fixing, minimal changes |
| 🧪 Testing | Meaningful tests without chasing 100% coverage |
| 📦 Git Workflow | Commit conventions, no push without approval |
| 🗂️ Project Context | Placeholder section to fill with project-specific details |

## 🛠️ Supported tools

The symlinks cover these tools:

| Symlink | Tool |
|---------|------|
| `CLAUDE.md` | Claude / Anthropic |
| `.github/copilot-instructions.md` | GitHub Copilot |
| `.cursorrules` | Cursor |

Any tool that reads `AGENTS.md` natively also works without a symlink. 🎉

## ⚖️ License

MIT — do whatever you want. Just don't blame me if your agent develops a
personality. 🤖✨