# agents.md

A universal `AGENTS.md` template that defines how AI coding agents must behave
across your projects. This repository is the single source of truth. Keep it
up to date, then copy `AGENTS.md` into any project that needs it.

## What is AGENTS.md?

`AGENTS.md` is a markdown file that gives AI coding agents the context and rules
they need to work in your codebase. It is injected into every agent session,
so every instruction matters. This repo maintains a shared, project-agnostic
version you can copy into any project.

## How to use it

1. Copy `AGENTS.md` to the root of your project.
2. Create symlinks so each tool reads the same file:

   ```bash
   ln -s AGENTS.md CLAUDE.md
   ln -s AGENTS.md .github/copilot-instructions.md
   ln -s AGENTS.md .cursorrules
   ```

3. Add project-specific sections at the bottom (tech stack, build commands,
   test commands, project structure). The universal rules stay the same.

## What it covers

| Section | What it does |
|---------|-------------|
| Language and Tone | STE rules, brevity, honesty, depth of explanation |
| Code Style | Naming, constants, enums, indentation, spacing |
| Comments | When to comment, style, do-not-touch rule |
| Architecture | Layered hierarchy, abstraction, member visibility |
| Workflow | Understand before act, verification, bug fixing, minimal changes |
| Testing | Meaningful tests without chasing 100% coverage |

## Supported tools

The symlinks cover these tools:

- `CLAUDE.md` — Claude / Anthropic
- `.github/copilot-instructions.md` — GitHub Copilot
- `.cursorrules` — Cursor

Any tool that reads `AGENTS.md` natively also works without a symlink.