# AGENTS.md

Universal rules for AI coding agents. Copy this file to any project.
Add project-specific sections at the bottom (see "Project Context" below).

---

## Boundaries

### Always

- Follow the language and code rules in this file.
- Run the project linters and formatters before you commit.
- Run the test suite before you claim a task is done.

### Ask First

- Before you change any access modifier from private to internal or public.
- Before you add, remove, or upgrade a dependency.
- Before you change CI/CD configuration or deployment settings.

### Never

- Commit secrets, API keys, or credentials.
- Touch code unrelated to the current task.
- Remove a test because it fails and you cannot fix it.
- Implement anything outside the agreed scope.

---

## Language and Tone

All text the agent produces follows ASD-STE100 Simplified Technical English
(STE). This includes chat responses, documentation, code comments, commit
messages, PR descriptions, and issue text.

### STE Rules

| Rule | Detail |
|------|--------|
| Short sentences | Max 20 words per instruction. Max 25 words per description. |
| One instruction per sentence | Never combine two instructions in one sentence. |
| Active voice | Prefer the active voice. Avoid passive constructions. |
| Simple verb forms | Use past, present, and future only. |
| One word, one meaning | Do not use synonyms for variation. Pick one term and keep it. |
| No idioms, metaphors, or slang | Use plain, literal language. |
| No em dashes | Use periods, commas, or parentheses instead. |
| Use articles | Use "the" and "a" where grammar permits. |

### Brevity

When writing for human consumption (comments, commit messages, replies),
use as few words as possible. Be direct. Less is more.

### Honesty Over Praise

Do not use superlatives. Do not praise the user or agree needlessly.
Give the cold, hard truth. State facts. Correct errors when you see them.

### Depth of Explanation

When the user asks for an explanation, proceed at increasing levels of depth:

1. Simple analogy or plain-English summary.
2. Technical explanation with project context.
3. Full deep dive into internals and edge cases.

Do not assume the user knows project-specific concepts. Introduce them before
you use them. Offer a deeper dive when it makes sense. Do not force it.

---

## Code Style

Where a linter or formatter can enforce a rule, let it.
These rules cover what linters cannot: design decisions, naming clarity,
and communication style.

### Naming

- Keep function names short: under 30 characters.
- Strive for clear, comprehensive names.

### Constants and Enums

- Extract recurring or meaningful values into named `const` or `enum`.
- Keep self-explanatory, one-off values inline to avoid clutter.
- If a value comes from a spec (e.g. HTTP 200 OK), use a constant regardless.
- Do not use magic numbers or magic strings.

### Enums Over Booleans

Use enums instead of booleans for function parameters.

```
// Do
fn set_mode(mode: Mode) // Mode::ReadOnly, Mode::ReadWrite

// Do not
fn set_mode(read_only: bool)
```

### Indentation and Control Flow

Reduce code indentation. Use early `return` and `continue`.
Keep nesting shallow.

```
// Do
fn find_user(id: u32) -> Option<User> {
    let user = db.get(id)?;

    if user.is_active() {
        return Some(user);
    }

    None
}

// Do not
fn find_user(id: u32) -> Option<User> {
    if let Some(user) = db.get(id) {
        if user.is_active() {
            return Some(user);
        }
    }

    None
}
```

### Spacing

Add empty lines between logical blocks of code. Let the reader breathe.

### Comments

- Explain *what* a block does and *why* it exists.
- Add a comment only if the code is not obvious by reading it.
- Keep comments short. Use examples when they help.
- Propose ASCII drawings to explain complete systems or complex flows.
- Do not comment on code you did not create or modify.

---

## Architecture

### Layered Boundary Hierarchy

Each layer may communicate only with its immediate neighbor below it.

```
┌─────────────────────────────────┐
│  Presentation / UI / Controller  │
├─────────────────────────────────┤
│  Service / Business Logic       │
├─────────────────────────────────┤
│  Abstraction / Driver Layer      │
├─────────────────────────────────┤
│  Raw I/O / Hardware / Network   │
└─────────────────────────────────┘
```

- Never punch holes through layers.
- Controllers and UI must not call database queries, raw hardware drivers,
  or low-level network clients directly. Route through the service layer.

### Program to Abstractions

Encapsulate low-level mechanics (raw hardware I/O, sector parsing, direct
socket streams) in a dedicated driver or abstraction layer. Expose clean,
high-level APIs. Calling code works with domain concepts, not raw details.

### Member Visibility

- Keep all fields and functions private unless external access is strictly
  required by the design.
- Treat visibility changes as a breaking design shift.
- Ask the user before changing any access modifier from private to internal
  or public.

---

## Workflow

### Understand Before You Act

- Make sure you fully understand the requirements before you start.
- Ask questions until everything is clear.
- Do not implement anything outside the agreed scope.

### Verify Before You Start

Before starting a task, define how to verify that:

1. The task is done.
2. The task is done at an acceptable level of quality.

State the verification steps. When you are not sure how to verify, ask the
user for input. Do not start implementation until the criteria are clear.

### Bug Fixing

When fixing a bug, follow this sequence:

1. Write the test that reproduces the bug.
2. Observe the test failing.
3. Write the fix.
4. Observe the test passing.

### Minimal Changes

- Minimize the number of changed lines.
- Change only what the task requires.
- Do not reformat, refactor, or "improve" unrelated code.

---

## Testing

- Write meaningful tests that cover important parts of the software.
- Do not chase 100% coverage. Do not write trivial tests just to increase
  coverage.
- Strive for a balance: a well-tested codebase without excessive tests that
  add maintenance burden, complexity, and slow CI down.

---

## Git Workflow

- Write commit messages in STE. Keep them short and descriptive.
- Use the present tense and the imperative mood: "Add filter" not "Added
  filter" or "Adds filter".
- Prefix the subject line with the change type: `feat:`, `fix:`, `docs:`,
  `refactor:`, `test:`, `chore:`.
- Limit the subject line to 50 characters. Wrap the body at 72 characters.
- Explain *what* and *why* in the body, not *how*. The diff shows the how.
- Do not push without user approval.

---

## Project Context

> When you copy this file to a project, replace the placeholders below with
> project-specific details. Delete this block and the markers. Keep only
> what applies to the project. For large monorepos, place additional
> AGENTS.md files inside subproject directories.

### Tech Stack

<!-- e.g. Rust 1.75, TypeScript 5.3, PostgreSQL 16, React 18 -->

### Build Commands

<!-- e.g. cargo build, npm run build -->

### Test Commands

<!-- e.g. cargo test, npm test, pytest -v -->

### Lint and Format

<!-- e.g. cargo clippy, npm run lint, npx prettier --write -->

### Project Structure

<!-- Brief description of key directories and their purpose -->

### Security

<!-- Any security considerations specific to this project -->