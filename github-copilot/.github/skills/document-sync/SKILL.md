---
name: document-sync
description: 'Use after code changes to make sure behavior contracts and verification checklists still match the implementation. Identifies gaps where the code was updated but the docs were not, or where docs describe behavior that no longer exists. Keywords: vibe coding, documentation, sync, contract, verification, outdated, mismatch, non-technical, doc maintenance.'
argument-hint: 'What was just changed, e.g.: just fixed a bug in the save feature, just changed how login works, check all docs after this refactor'
user-invocable: true
---

# Document Sync Skill

After code changes, check that behavior contracts and verification checklists still accurately describe what the app does. Update any docs that are out of date.

## When to Use

- After fixing a bug that revealed a missing or incorrect contract item
- After a refactor that changed internal structure but should not have changed behavior
- After adding an edge case or error handling that was not in the original contract
- Periodically, to keep docs trustworthy as the project grows

## When Not to Use

- A new feature is being added — write the behavior contract BEFORE implementation, not after
- The user wants to add new behaviors — use `behavior-contract` skill instead

## Procedure

### Phase 1 — Identify what changed

Ask if not specified: "What was changed in the last session? I'll check if the docs still match."

Read:
- The relevant source files (or file diff if available)
- The current behavior contract in `docs/contracts/`
- The current verification checklist in `docs/verification/`

### Phase 2 — Compare code vs. docs

Check each contract item against the current implementation:

**A. Missing contract items** — code does something the contract does not mention
- New error messages added during bug fixing
- New validation rules added
- New edge case handling added
- Default values or fallbacks not documented

**B. Outdated contract items** — contract describes something that no longer exists
- Error messages that were changed or removed
- Behaviors that were modified during a fix
- Features that were removed or replaced

**C. Missing verification steps** — a behavior exists but there is no way to verify it
- New error cases with no test instructions
- Edge cases with no verification steps

**D. Verification steps that no longer work** — the steps reference old UI labels or flows
- Button names that changed
- Page routes that changed
- Form fields that were added or removed

### Phase 3 — Report the gaps

---
## Document Sync Report

Files reviewed: [list]

### Contract gaps

| Type | Location | Current doc says | What the code actually does |
|------|----------|-----------------|----------------------------|
| Missing | contract item # | (not documented) | [plain language description] |
| Outdated | contract item # | [what doc says] | [what code actually does] |

### Verification gaps

| Type | Location | Issue |
|------|----------|-------|
| Missing step | [feature] | [behavior with no verification] |
| Broken step | [step #] | [why it no longer works] |

---

### Phase 4 — Update the docs

For each gap found:
1. Show the user the proposed update in plain language
2. Confirm: "Should I update the contract to say [new text] instead of [old text]?"
3. Apply updates one item at a time

Do not silently rewrite entire documents — only update the specific items that changed.

### Phase 5 — Final check

After updates:
> "Docs are now in sync with the code. Summary:
> - [N] contract items updated
> - [N] verification steps updated
>
> The behavior contracts in `docs/contracts/` now accurately describe how the app works."

Suggest a commit:
> "Ready to save? Commit: `Sync docs after [what changed]`"
