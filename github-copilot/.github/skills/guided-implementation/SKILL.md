---
name: guided-implementation
description: 'Use when a behavior contract is confirmed and a non-technical user wants AI to implement the feature step by step with verification at each step. Prevents large unreviewed changes. Keywords: vibe coding, implement, build, code, step by step, non-technical, guided, verified implementation, small changes.'
argument-hint: 'Feature and contract to implement, e.g.: implement login per docs/contracts/login.md, build the invoice list feature'
user-invocable: true
---

# Guided Implementation Skill

Implement a feature step by step based on a confirmed behavior contract. Each step changes a small number of files, explains what changed in plain language, and provides verification the user can perform before proceeding.

## When to Use

- A behavior contract exists in `docs/contracts/` and is confirmed (no unresolved TBD items blocking implementation)
- The user wants to proceed with building the feature
- The user does not know code and needs plain-language explanations and verification at each step

## When Not to Use

- No behavior contract exists yet — use `behavior-contract` skill first
- The idea is still vague — use `Clarify My Requirement` prompt first
- A bug needs fixing — use `Diagnose and Fix` prompt

## Procedure

### Phase 1 — Pre-flight check

1. **Read existing context.** Read the behavior contract from `docs/contracts/`. Read workspace instructions. Read any existing design docs in `docs/design/`. Read `docs/contracts/product-definition.md` if it exists.

2. **Check for TBD items.** If the behavior contract has unresolved TBD items, ask the user to resolve them first. Do not implement behaviors marked TBD.

3. **Check project state.** Confirm the project can run (build succeeds, app starts). If the project is not yet initialized, tell the user this must happen first and stop.

4. **Plan the steps.** Break the implementation into steps. Each step:
   - Changes no more than 3 files
   - Delivers one testable behavior from the contract
   - Can be verified by the user through a specific UI operation

   Present the step plan to the user in plain language:
   > "I'll implement this feature in N steps:
   > 1. [what step 1 does, in plain language]
   > 2. [what step 2 does]
   > ...
   > After each step I'll tell you how to check it. Want me to start?"

   Wait for user approval.

### Phase 2 — Step-by-step implementation

For each step:

1. **Announce what you will change.**
   > "Step N: I'm going to [plain language description]. This will change [file names]."

2. **Make the code changes.** Change no more than 3 files per step.

3. **Explain what you changed.**
   > "I changed [file] to [plain language description of what the change does]. This means [how the user will notice the change]."
   > "I did NOT touch [mention any related files that were left alone]. Those still work the same as before."

4. **Provide verification steps.**
   > "To check this is working:
   > 1. [Open / Go to ...]
   > 2. [Do ...]
   > 3. You should see [...]
   > 4. Also try [error/edge case from contract]: you should see [...]"

5. **Wait for user feedback.** Do not proceed to the next step until the user confirms:
   - "It works" → proceed to next step
   - "Something is wrong" → switch to diagnosis mode (Step 2 of `Diagnose and Fix` protocol): explain what might be wrong, propose minimal fix, get approval
   - "I want to change the requirement" → update the behavior contract first, then adjust the plan

### Phase 3 — Wrap up

After all steps are complete:

1. **Full verification.** Provide a consolidated verification checklist covering ALL behaviors from the contract. Ask the user to run through it end to end.

2. **Save verification checklist.** Save the verification steps to `docs/verification/<feature-name>.md`.

3. **Git commit.** Suggest committing the working state:
   > "This feature is complete and verified. I recommend saving this version. Should I create a save point (Git commit)?"

4. **Update contract status.** Update `docs/contracts/<feature-name>.md` status to "Implemented and verified" with the current date.

5. **State next step.** Recommend what to do next:
   - Implement the next MVP feature (refer to product definition)
   - Or note any dependencies or issues found during implementation

## Hard Rules

- **Maximum 3 files per step.** If a step requires more, split it further.
- **No silent changes.** Every file change must be announced and explained in plain language.
- **No scope creep.** Only implement behaviors listed in the confirmed contract. If you notice a missing behavior, tell the user and offer to update the contract — do not silently add it.
- **No refactoring during implementation.** Do not reorganize, rename, or restructure existing code unless it is necessary to implement the current contract behavior. If refactoring seems needed, explain why and get user approval.
- **Verification before next step.** Never skip to the next step without user confirmation that the current step works.
- **3-attempt limit on fixes.** If a step fails verification 3 times, follow the escalation protocol from `Diagnose and Fix`: revert, restart, or open new conversation.
