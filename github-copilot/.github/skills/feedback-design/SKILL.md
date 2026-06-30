---
name: feedback-design
description: 'Use to design and implement all the feedback states an interface needs: loading indicators, success messages, error messages, empty states, and confirmation dialogs. Prevents the "nothing happened" confusion and silent failures. Keywords: vibe coding, feedback, loading, success, error, empty state, toast, notification, spinner, UX, non-technical.'
argument-hint: 'What to design feedback for, e.g.: the save button, the whole app, the delete confirmation, the empty customer list'
user-invocable: true
---

# Feedback Design Skill

Design and implement all the feedback states users need to understand what is happening: loading, success, error, empty, and confirmation. Eliminates "did it work?" confusion.

## When to Use

- Users are clicking buttons and not knowing if anything happened
- There are no loading indicators when operations take time
- Success and error messages are missing or inconsistent
- Empty lists/states show nothing instead of a helpful message
- Destructive actions (delete, cancel) have no confirmation step

## When Not to Use

- A specific error message is wrong — use `Diagnose and Fix` prompt
- The issue is a functional bug, not a missing feedback state

## The Five Feedback States

Every significant user action needs all applicable states designed:

| State | When | Example |
|-------|------|---------|
| **Loading** | Operation is in progress | Spinner on Save button while saving |
| **Success** | Operation completed | "Customer saved!" toast message |
| **Error** | Operation failed | "Failed to save. Please try again." |
| **Empty** | No data to show | "No customers yet. Add your first one." |
| **Confirmation** | Destructive or irreversible action | "Delete this customer? This cannot be undone." |

## Procedure

### Phase 1 — Audit existing feedback

Scan the app for actions that are missing feedback states:

For each significant action (save, delete, submit, load), check:
- [ ] Is there a loading state while the operation runs?
- [ ] Is there a clear success message?
- [ ] Is there a clear error message if it fails?
- [ ] For delete/destructive: is there a confirmation step?

For each data list or display area, check:
- [ ] What is shown when the list is empty?
- [ ] What is shown while loading?
- [ ] What is shown if loading fails?

### Phase 2 — Design each feedback state

For each missing state, design:

**Loading state:**
- Show a spinner inside the button, or a full-screen overlay, or a skeleton placeholder?
- Disable the button/form while loading to prevent double-submission?
- Is there a timeout? What if it takes more than 10 seconds?

**Success state:**
- Toast notification (appears briefly, then disappears): best for non-critical confirmations
- Inline message (stays visible): best for form submissions
- Redirect to another page: best when the workflow is complete

Success message format: "[What was done]. [Optional next step]"
Example: "Customer saved. You can now create an invoice for them."

**Error state:**
- Is this a user error (they did something wrong) or a system error (server/network problem)?
- User errors: show inline next to the relevant field, explain what to fix
- System errors: show a non-technical message + retry option
- Never show raw error codes to users

Error message format: "[What went wrong in plain language]. [What the user can do]"
Example: "We couldn't save this customer. Check your internet connection and try again."

**Empty state:**
- What is shown when a list has no items?
- Include: an illustration or icon (optional), a brief explanation, a call-to-action button
- Empty state message format: "[Why it's empty]. [What to do]"
- Example: "You haven't added any customers yet. Add your first customer to get started." [Add Customer button]

**Confirmation dialog:**
- Required for: delete, cancel (if data will be lost), irreversible actions
- Must clearly state: what will be deleted/cancelled, that it cannot be undone
- Buttons: clear labels ("Delete Customer" not just "OK"), destructive button in red
- Format: "Are you sure you want to [action]? [What will be affected]. This cannot be undone."

### Phase 3 — Implement

For each feedback state, one at a time:
1. Announce what you are adding and where
2. Change no more than 3 files
3. Explain what the user will see
4. Verify: "To test: click [button]. While it is processing, you should see [loading state]. After it completes, you should see [success/error message]."
5. Wait for confirmation

### Phase 4 — Consistency check

After implementing individual states, check for consistency across the app:
- Are all success messages the same style? (same position, duration, color)
- Are all error messages the same style?
- Are all loading spinners the same?
- Do all delete confirmations look the same?

If not, standardize them.

### Phase 5 — Save

> "Feedback states implemented. Commit: `Add loading/success/error/empty states to [area]`"

Update the behavior contract for each affected feature with the new feedback states documented.
