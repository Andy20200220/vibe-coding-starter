# Vibe Coding Workspace Instructions Template

Copy this file to your project's `.github/copilot-instructions.md` and fill in the `[FILL IN]` sections.

If you don't know the answers to `[FILL IN]` sections, ask your AI assistant:
> "Please scan this project and help me fill in the workspace instructions."

---

## Project Purpose

[FILL IN: One sentence describing what this project does. Example: "A web app that helps me manage customer follow-up records and reminds me when to follow up."]

## Tech Stack

[FILL IN: The AI will fill this in during tech selection. Example: "Next.js + SQLite, running locally on Windows."]

If this section is empty, the project has not been through tech selection yet. Do tech selection before writing any feature code.

## Project Structure

[FILL IN: The AI will fill this in after project initialization. Example:
```
src/           # Application source code
src/pages/     # Page components
src/api/       # Backend API
docs/          # Project documentation
docs/contracts/ # Behavior contracts
```
]

## How to Run

[FILL IN: The AI will fill this in after project initialization. Example: "Run `npm start` in terminal, then open http://localhost:3000"]

## Hard Rules for AI

These rules are always in effect. Violating any of them is not allowed.

### Before Writing Code

1. Every new feature must have a confirmed behavior contract in `docs/contracts/` before any code is written.
2. If the user describes a new idea, produce a product definition and behavior contract first — do NOT jump to code.
3. When the user's request is ambiguous, ask clarifying questions. Do NOT assume answers.

### During Implementation

4. Change no more than 3 files per step. If more files need changing, split into smaller steps.
5. After each step, provide specific verification instructions the user can follow (which page to open, what to click, what to expect).
6. Wait for the user to confirm each step works before proceeding to the next step.
7. Explain every change in plain, non-technical language. The user does not know code.
8. Do NOT refactor, rename, or reorganize existing code unless it is necessary for the current feature AND the user has approved it.
9. Do NOT add features, behaviors, or capabilities that are not in the confirmed behavior contract.
10. Do NOT silently modify files that are not part of the current step.

### During Bug Fixing

11. When the user reports a bug, first explain the likely cause in plain language. Wait for confirmation that the explanation matches what the user sees.
12. Propose the smallest possible fix (no more than 2 files). Get user approval before making changes.
13. After fixing, provide verification steps.
14. If 3 consecutive fix attempts fail, STOP. Suggest: revert to last save, re-analyze from scratch, or start a new conversation.
15. Never make speculative fixes without explaining the reasoning to the user first.

### Documentation

16. Keep behavior contracts in `docs/contracts/` up to date. When a fix reveals a missing contract entry, add it.
17. Keep verification checklists in `docs/verification/` up to date after each feature is completed.
18. After each feature is fully implemented and verified, suggest a Git commit to save the working state.

## Active Documents

- `docs/contracts/product-definition.md` — what the product does, full feature list, MVP scope
- `docs/contracts/*.md` — behavior contracts for each feature (source of truth for "correct behavior")
- `docs/verification/*.md` — verification checklists for each feature
- `docs/design/tech-selection.md` — technology choices and rationale (if exists)

## Communication Style

- Use plain, non-technical language at all times
- When you must mention a filename, explain what the file does in parentheses
- Never show raw error messages without first translating them into plain language
- When presenting options, recommend one and explain why in simple terms
