---
name: git-commit
description: 'Use when a non-technical user has completed and verified a feature or fix and wants to save their progress using Git. Generates a meaningful commit message, checks what changed, and commits without requiring the user to know any Git commands. Keywords: vibe coding, git, commit, save, progress, checkpoint, non-technical, version control.'
argument-hint: 'What was just completed, e.g.: finished the login feature, fixed the save button bug, completed behavior contract for invoice list'
user-invocable: true
---

# Git Commit Skill

Save the current project state as a Git checkpoint after a feature or fix is completed and verified. Handles everything — the user only needs to confirm.

## When to Use

- A feature has been fully implemented and verified by the user
- A bug has been fixed and verified
- A behavior contract or important document has been completed
- The user wants to create a save point before starting something risky

## When Not to Use

- The current step has not been verified yet — verify first, then save
- The user is in the middle of implementing a feature — save only complete, working states
- There is nothing new to save

## Procedure

### Phase 1 — Check what changed

1. Run `git status` to see all changed files.
2. Run `git diff --stat` to see a summary of what was modified.
3. Identify any files that should NOT be committed:
   - Files with secrets or API keys
   - Temporary files, log files, build outputs
   - Files the user did not intend to change
   - Files already covered by `.gitignore` (these won't appear, but check if `.gitignore` exists and is reasonable)

### Phase 2 — Report to the user

Present a plain-language summary of what will be saved:

> "Here's what I'm about to save:
>
> **New files added:**
> - `[filename]` — [what this file does in plain language]
>
> **Files changed:**
> - `[filename]` — [what changed in plain language]
>
> **Nothing I'd exclude** (or: "I'd suggest NOT including `[file]` because [reason]")
>
> Does this look right?"

Wait for user confirmation. If the user spots something wrong, do not commit — investigate first.

### Phase 3 — Generate a commit message

Write a commit message that:
- Starts with a verb (Add, Fix, Update, Remove, Improve)
- Describes what changed in plain English — not technical jargon
- Is one line, under 72 characters if possible
- Optionally has a short body if there is important context

Examples of good messages:
- `Add login feature with phone number verification`
- `Fix save button not responding on the invoice page`
- `Update customer list to show last contact date`
- `Add behavior contract for the notification feature`

Show the message to the user:
> "Commit message: `[message]`
> Is this OK, or would you like to change it?"

Wait for confirmation.

### Phase 4 — Commit

After user confirms both the file list and the message:

1. Stage all intended files: `git add [files]`
2. Commit: `git commit -m "[message]"`
3. Report the result:

> "Saved! ✓
>
> Your progress is now saved as: `[commit message]`
>
> If something goes wrong later, you can always come back to this point by telling me: 'Help me go back to the version before [what you just did].'"

### Phase 5 — Offer next step

> "What would you like to work on next?"

---

## If Git Is Not Set Up

If `git status` fails because Git is not initialized:

1. Tell the user: "Git isn't set up for this project yet. Git is like a save system — it keeps a history of every version of your project so you can go back if something breaks. Should I set it up now?"
2. If yes: run `git init`, create a basic `.gitignore` appropriate for the project's tech stack, make an initial commit with message `Initial project setup`.
3. Then proceed with the normal commit procedure.
