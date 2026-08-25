# AGENTS.md

This file is the source of truth for how AI agents must behave in this project.
Copy it to other repositories when needed.

---

## 1. Language and Tone

All text the agent produces follows ASD-STE100 Simplified Technical English (STE).
This includes chat responses, documentation, code comments, commit messages,
PR descriptions, and issue text.

### 1.1 Core STE Rules

| Rule | Detail |
|------|--------|
| Short sentences | Max 20 words per instruction. Max 25 words per description. |
| One instruction per sentence | Never combine two instructions in one sentence. |
| Active voice | Prefer the active voice. Avoid passive constructions. |
| Simple verb forms | Use past, present, and future only. |
| One word, one meaning | Do not use synonyms for variation. Pick one term and keep it. |
| No idioms, metaphors, or slang | Use plain, literal language. |
| Use articles | Use "the" and "a" where grammar permits. |

### 1.2 Brevity

When writing for human consumption (comments, commit messages, replies),
use as few words as possible. Pick every word meticulously. Reduce volume
to a strict minimum. Be direct. Less is more.

### 1.3 Honesty Over Praise

Do not use superlatives. Do not praise the user or agree needlessly.
Give the cold, hard truth. State facts. Correct errors when you see them.

### 1.4 Depth of Explanation

When the user asks for an explanation, proceed at increasing levels of depth:

1. Start with a simple, high-level summary. Explain it like the user is five.
2. Add a first layer of analysis. Connect the concept to the current project.
3. Offer to go deeper if the topic warrants it.

Do not assume the user knows project-specific concepts. Introduce them before
you use them. Suggest a deep dive when it makes sense, but do not force it.

```
Depth 1:  Simple analogy or plain-English summary.
Depth 2:  Technical explanation with project context.
Depth 3:  Full deep dive into internals and edge cases.
```

---

## 2. Code Style

### 2.1 Naming

- Keep function names short: under 30 characters.
- Strive for clear, comprehensive names.
- Use descriptive constants and enums. Do not use magic numbers or magic strings.

### 2.2 Constants and Enums

- Extract recurring or meaningful values into named `const` or `enum`.
- Keep self-explanatory, one-off values inline to avoid clutter.
- If a value comes from a spec (e.g. HTTP 200 OK), use a constant regardless.

### 2.3 Enums Over Booleans

Use enums instead of booleans for function parameters.

```
// Do
fn set_mode(mode: Mode) // Mode::ReadOnly, Mode::ReadWrite

// Do not
fn set_mode(read_only: bool)
```

### 2.4 Indentation and Control Flow

- Reduce code indentation. Use early `return` and `continue`.
- Keep nesting shallow.

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

### 2.5 Breathing Room

Add empty lines between logical blocks of code. Let the reader breathe.

---

## 3. Comments

### 3.1 When to Comment

- Explain *what* a block does and *why* it exists.
- Add a comment only if the code is not obvious by reading it.
- Do not over-comment. Add only what a future human needs to understand the code.

### 3.2 Style

- Keep comments short and to the point.
- Use examples when they help.
- Propose ASCII drawings to explain complete systems or complex flows.

### 3.3 Do Not Touch What You Did Not Change

Do not add comments to code you did not create or modify.
Do not touch blocks of code unrelated to the current task.

---

## 4. Architecture

### 4.1 Layered Boundary Hierarchy

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
- Controllers and UI components must not call database queries, raw hardware
  drivers, or low-level network clients directly.
- Always route through the intermediate service or abstraction layer.

### 4.2 Program to Abstractions

Encapsulate low-level mechanics (raw hardware I/O, sector parsing, direct
socket streams) in a dedicated driver or abstraction layer.
Expose clean, high-level APIs to the rest of the application.
Calling code works with domain concepts, not raw implementation details.

### 4.3 Member Visibility

- Keep all fields and functions private unless external access is strictly
  required by the design.
- Treat visibility changes as a breaking design shift.
- Prompt the user for explicit approval before changing any access modifier
  from private to internal or public.

---

## 5. Workflow

### 5.1 Understand Before You Act

- Make sure you fully understand the requirements before you start.
- Ask questions until everything is clear.
- Do not implement anything outside the agreed scope.

### 5.2 Bug Fixing

When fixing a bug, follow this sequence:

1. Write the test that reproduces the bug.
2. Observe the test failing.
3. Write the fix.
4. Observe the test passing.

### 5.3 Verification Before Starting

Before starting a task, define how to verify that:

1. The task is done.
2. The task is done at an acceptable level of quality.

State the verification steps. When you are not sure how to verify, ask the user
for input. Do not start implementation until the verification criteria are clear.

### 5.4 Minimal Changes

- Minimize the number of changed lines when implementing a feature.
- Change only what the task requires.
- Do not reformat, refactor, or "improve" unrelated code.

---

## 6. Testing

- Tests are important. Write meaningful tests that cover important parts of the
  software.
- Do not chase 100% coverage. Do not write trivial tests just to increase
  coverage.
- Strive for a balance: a well-tested codebase without excessive tests that add
  maintenance burden, complexity, and slow CI down.