# AGENTS.md

Opinionated rules for AI coding agents. Adapt this file to each project.
Add project-specific sections at the bottom (see "Project Context" below).

---

## Boundaries

### Always

- Follow the language and code rules in this file.
- Inspect the worktree before you edit files. Preserve existing user changes.
- Run relevant project checks before you claim a task is done.
- Report each validation command and its result.

### Ask First

- Before you change any access modifier from private to internal or public.
- Before you add, remove, or upgrade a dependency.
- Before you change CI/CD configuration or deployment settings.
- Before you perform a destructive operation that can discard data or local changes.
- Before you deploy, publish, release, or send an external message without explicit authorization.

### Never

- Commit or expose secrets, API keys, or credentials.
- Add a `Co-authored-by` or equivalent attribution trailer to a commit.
- Add agent attribution, agent signatures, or agent promotional text to project content or repository metadata.
- Overwrite or discard changes that you did not create.
- Use destructive Git commands to remove existing work.
- Disable a security control or a test to make a task pass.
- Touch code unrelated to the current task.
- Remove a test because it fails and you cannot fix it.
- Implement anything outside the agreed scope.

---

## Language and Tone

All text the agent produces uses the STE-inspired writing rules in this file.
These rules adapt ASD-STE100 principles to this template's preferences.
They do not claim full ASD-STE100 compliance.
This scope includes chat responses, documentation, code comments, commit
messages, PR descriptions, and issue text.

### AI Wording

Use common, literal, task-specific wording.
Every sentence must add something the reader does not already have.
Do not add facts, names, or numbers that do not come from the code,
a source, or the user.

| Pattern | Avoid | Do instead |
|---------|-------|------------|
| Staged contrast | "It's not a cache, it's a strategy." | State the claim. Keep a contrast only if the reader believes the wrong half. |
| One-line closer | "That is the real win." | Cut it, or replace it with a new fact. |
| Run-up | "Let's dive in." "Here's the thing." | Start with the point. |
| Arguing with no one | "To be clear, this isn't about X." | Remove it. |
| Forced triad | "fast, reliable, and scalable" | List only items that each mean something different. |
| Inflated significance | "marks a pivotal shift" | Keep the fact. Drop the claim of importance. |
| Long verb phrase | "serves as", "boasts", "features" | Use "is" or "has". |
| Stacked hedges | "could potentially" | Use one qualifier, and only when the doubt is real. |
| Self-description | "This function was added to replace..." | Say what the code does now. Put history in commits. |
| Chat leftovers | "Great question!" "I hope this helps." | Remove them from commits, docs, and PR text. |
| Decorative format | A bold label on every list item, emojis, a rule between every section | Use bold only for a real warning. |

In replies, lead with the answer or decision.
Do not restate context the user already gave.

Do not use superlatives. Do not praise the user or agree needlessly.
State facts directly. Correct errors when you see them.

Avoid terms such as `load-bearing`, `seam`, `halves`, `leg`, `quietly`,
`byte-identical`, `ruling`, `genuine`, `delve`, `crucial`, `pivotal`,
`robust` (figurative), `seamless`, `leverage`, `showcase`, `underscore`,
`landscape` (abstract), `testament`, and `additionally` when a plain
alternative preserves the meaning.

### STE-Inspired Writing Rules

| Rule | Detail |
|------|--------|
| Short sentences | Max 20 words per instruction. Max 25 words per description. Vary sentence length within the cap. |
| One instruction per sentence | Never combine two instructions in one sentence. |
| Active voice | Prefer the active voice. Avoid passive constructions. |
| Simple verb forms | Use past, present, and future only. |
| One word, one meaning | Do not use synonyms for variation. Pick one term and keep it. |
| No idioms, metaphors, or slang | Use plain, literal language. |
| No em dashes | Use periods, commas, or parentheses instead. |
| Use articles | Use "the" and "a" where grammar permits. |

### Brevity

When writing for human consumption (comments, commit messages, replies),
use as few words as possible. Be direct. Remove unnecessary words.

### Depth of Explanation

When the user asks for an explanation, proceed at increasing levels of depth:

1. Plain-English summary.
2. Technical explanation with project context.
3. Detailed explanation of internals and edge cases.

Do not assume the user knows project-specific concepts. Introduce them before
you use them. Offer more detail when it helps. Do not force it.

---

## Code Style

Use a linter or formatter for rules that it can enforce.
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

Add empty lines between logical blocks of code.

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

- Do not bypass a layer.
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

- Read the task and all applicable instructions before you start.
- Proceed with reasonable assumptions for low-risk, reversible work.
- State each material assumption before it affects the result.
- Ask when a choice affects behavior, security, data, dependencies, public APIs, or scope.
- Do not implement anything outside the agreed scope.

### Verify Before You Start

Before editing, identify how to verify that:

1. The task is done.
2. The task is done at an acceptable level of quality.

State the verification steps for non-trivial or high-risk tasks.
Use checks that match the changed area and the task risk.
Run the full required suite when the project requires it and the environment permits it.
If a required check is unavailable, report the exact command and the reason.
Report unrelated failures without fixing them.

### Bug Fixing

When practical, fix a bug with this sequence:

1. Write an automated regression test that reproduces the bug.
2. Observe the test failing.
3. Write the fix.
4. Observe the test passing.

When an automated test is impractical, use the smallest reliable reproduction.
Explain why you did not add a test.

### Minimal Changes

- Minimize the number of changed lines.
- Change only what the task requires.
- Do not reformat, refactor, or "improve" unrelated code.
- Use formatter check mode or format only the changed files.

---

## Testing

- Write meaningful tests that cover important parts of the software.
- Do not require 100% coverage. Do not write trivial tests only to increase
  coverage.
- Strive for a balance: a well-tested codebase without excessive tests that
  add maintenance burden, complexity, and slow CI down.

---

## Git Workflow

- Write commit messages with the STE-inspired rules. Keep them short and descriptive.
- Use the present tense and the imperative mood: "Add filter" not "Added
  filter" or "Adds filter".
- Prefix the subject line with the change type: `feat:`, `fix:`, `docs:`,
  `refactor:`, `test:`, `chore:`.
- Limit the subject line to 50 characters. Wrap the body at 72 characters.
- Explain *what* and *why* in the body, not *how*. The diff shows the how.
- Do not create a commit unless the user requests it.
- Ask for approval immediately before each push.
- Do not create or modify issues, pull requests, releases, deployments, or external messages without explicit authorization.

---

## Project Context

> When you copy this file to a project, replace the placeholders below with
> project-specific details. Delete this block and all placeholder comments.
> Remove unused headings. For large monorepos, add instructions inside each
> subproject when the selected agent supports nested instruction files.

### Project Overview

<!-- Explain the project purpose, primary users, and important domain terms. -->

### Tech Stack

<!-- e.g. Rust 1.75, TypeScript 5.3, PostgreSQL 16, React 18 -->

### Setup Commands

<!-- List dependency installation and first-run commands. Include working directories. -->

### Environment

<!-- List required tools, environment variables, services, and operating-system limits. Never include secret values. -->

### Package Manager

<!-- Name the required package manager and lockfile policy. -->

### Build Commands

<!-- e.g. cargo build, npm run build -->

### Type Check

<!-- List type-check commands when the build command does not include them. -->

### Test Commands

<!-- e.g. cargo test, npm test, pytest -v -->

### Lint and Format

<!-- e.g. cargo clippy, npm run lint, npx prettier --write -->

### Validation Matrix

<!-- Map each change type to the minimum required checks. Include full-suite requirements. -->

### Project Structure

<!-- Brief description of key directories and their purpose -->

### Generated and Vendor Files

<!-- Identify generated, vendored, or locked files. Explain how to update them safely. -->

### Public APIs and Data Changes

<!-- Define public API boundaries, compatibility rules, migrations, and data safety requirements. -->

### Known Limitations

<!-- List known failing checks, environment restrictions, and accepted technical constraints. -->

### Security

<!-- Define secret handling, authentication rules, sensitive data, and prohibited operations. -->
