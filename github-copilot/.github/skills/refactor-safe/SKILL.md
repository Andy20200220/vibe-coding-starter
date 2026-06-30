---
name: refactor-safe
description: 'Use when the project has become slow to change, has messy or duplicated code, or the user wants to clean up without breaking existing features. Makes structural improvements in small verified steps without changing any user-visible behavior. Keywords: vibe coding, refactor, clean up, technical debt, reorganize, improve, non-technical, safe, no behavior change.'
argument-hint: 'What to clean up, e.g.: the whole project feels messy, the login file is too long, there is a lot of duplicated code'
user-invocable: true
---

# Safe Refactor Skill

Clean up and improve code structure without changing any user-visible behavior. Every step is small, explained in plain language, and verified to ensure existing features still work.

## When to Use

- Adding new features has become slower than expected
- The same bug keeps appearing in multiple places
- Code files have grown very large
- The user (or AI) keeps having to explain the same workarounds
- A `project-health-check` identified maintainability issues

## When Not to Use

- There is an active bug to fix — fix the bug first
- The user wants to add a new feature — add it first, clean up separately
- The project is in a broken state — restore it to working first

## Core Rule

**Refactoring must never change what the user sees or experiences.** Every behavior that worked before must work exactly the same after. If a behavior contract exists, all items must still pass after each step.

## Procedure

### Phase 1 — Understand the scope

Ask the user what feels problematic if not specified. Look for:
- Very large files (suggest splitting)
- Duplicated logic in multiple places (suggest extracting a shared function)
- Hard-to-follow naming (suggest rename)
- Mixed concerns in one file (suggest separation)
- Unused code (suggest deletion)

Run `project-health-check` first if the scope is unclear.

### Phase 2 — Plan the refactor

Break the work into the smallest possible independent steps. Each step:
- Changes no more than 3 files
- Does exactly one type of cleanup (rename, extract, move, delete)
- Can be verified by running existing tests or manually checking the UI

Present the plan in plain language:
> "Here's what I'd like to clean up, in [N] steps:
> 1. [Plain language description — what it is now, what it will be after]
> 2. ...
>
> **Important:** None of these changes will add new features or change how anything works. I'm only changing how the code is organized internally.
>
> Shall I start?"

Wait for approval.

### Phase 3 — Execute step by step

For each step:

1. **Announce what you will do** — and explicitly state what will NOT change:
   > "Step N: I'm going to [description]. The [feature name] will still work exactly the same. I'm NOT changing [related thing]."

2. **Make the change** — maximum 3 files.

3. **Explain the result in plain language:**
   > "Done. [What changed and why it is better now, in plain language.]"

4. **Verify nothing broke:**
   - If automated tests exist: run them. All must still pass.
   - If no automated tests: provide manual verification steps covering the affected feature.
   > "To confirm everything still works:
   > 1. [Open / Go to ...]
   > 2. [Do ...]
   > 3. You should still see [...] — same as before"

5. **Wait for confirmation** before the next step.

### Phase 4 — Wrap up

After all steps:
> "The cleanup is complete. Here's a summary of what changed:
> - [Before → After, in plain language]
> - [Before → After]
>
> The project should now be easier to change going forward because [plain language explanation]."

Suggest a git commit:
> "Ready to save? I'd commit this as: `Refactor [area] for easier maintenance`"
