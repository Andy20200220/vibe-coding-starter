---
name: code-review
description: 'Use when a feature has been implemented and the user wants a plain-language code quality review before moving on. Checks for bugs, security issues, performance problems, and maintainability issues without requiring the user to read code. Keywords: vibe coding, code review, quality, security, bug, risk, non-technical, audit, check.'
argument-hint: 'What to review, e.g.: the login feature, the files changed in the last step, the entire project'
user-invocable: true
---

# Code Review Skill

Review recently implemented code for quality, security, and correctness. Report findings in plain language with clear severity ratings.

## When to Use

- A feature has just been implemented (after `guided-implementation` skill)
- Before a feature is considered "done"
- When something feels wrong but there is no obvious bug yet
- Before adding a new feature on top of existing code

## When Not to Use

- A specific bug has already been reported — use `Diagnose and Fix` prompt instead
- The user wants a full project overview — use `project-health-check` skill instead

## Procedure

### Phase 1 — Identify scope

Ask the user what to review if not specified:
> "Should I review: (a) just the files changed in the last feature, (b) a specific feature area, or (c) the whole project?"

Read the relevant source files. Also read the corresponding behavior contract in `docs/contracts/` to understand what the code is supposed to do.

### Phase 2 — Run the review

Check each category below. Rate each issue: 🔴 Must fix / 🟡 Should fix / 🟢 Nice to fix.

**A. Correctness — does the code do what the contract says?**
- Does each behavior in the contract have a corresponding implementation?
- Are there behaviors in the contract that are missing or incomplete?
- Are there behaviors implemented that are NOT in the contract?

**B. Security — could this be exploited or expose data?**
- Are there hardcoded passwords, tokens, or API keys in source files?
- Is user input validated before being used? (prevents injection attacks)
- Are error messages exposing internal system details to users?
- Is sensitive data (passwords, personal info) stored or logged in plain text?
- Are file paths or system commands constructed from user input?

**C. Error handling — what happens when things go wrong?**
- Are errors caught and handled, or do they silently crash the app?
- Does the user see a helpful message when something fails?
- Are there empty error handlers that hide failures?

**D. Edge cases — does the code handle unusual inputs?**
- What happens with empty input, zero values, very large values?
- What happens if a database query returns nothing?
- What happens if a network request fails?

**E. Maintainability — will this be easy to change later?**
- Are there very large functions (over ~50 lines) that do too many things?
- Is the same logic repeated in multiple places?
- Are variable and function names meaningful?
- Are there TODO/FIXME comments from the implementation that were not resolved?

### Phase 3 — Produce the report

Present findings in plain language:

---
## Code Review Report

Feature reviewed: [name]
Files reviewed: [list]

### What Looks Good
- [Item]

### Issues Found

#### 🔴 Must Fix
| # | Issue | Plain language explanation | How to fix |
|---|-------|--------------------------|------------|
| 1 | ... | ... | ... |

#### 🟡 Should Fix
| # | Issue | Plain language explanation | How to fix |
|---|-------|--------------------------|------------|

#### 🟢 Nice to Fix
| # | Issue | Plain language explanation | How to fix |
|---|-------|--------------------------|------------|

### Verdict
[One sentence: ready to ship / fix blockers first / significant rework needed]

---

### Phase 4 — Offer to fix

> "Would you like me to fix the 🔴 must-fix items now? I'll do them one at a time and verify each one."

If yes, address each issue using the `guided-implementation` flow: announce the change, make it, explain it, provide verification steps, wait for confirmation.
