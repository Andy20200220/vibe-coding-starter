---
name: "Diagnose and Fix"
description: 'Use when a non-technical user encounters a bug or unexpected behavior and needs structured diagnosis before any code changes. Prevents guess-and-patch spirals. Keywords: vibe coding, bug, fix, broken, not working, error, diagnose, non-technical, debug.'
argument-hint: 'Describe what you see, e.g.: I clicked the save button but nothing happened, the page shows an error after login, the list is empty when it should have data'
agent: "agent"
---

Help a non-technical user diagnose and fix a bug with a structured protocol that prevents guess-and-patch spirals.

Requirements:

- Read [workspace instructions](../copilot-instructions-template.md).
- The user does not know code. All explanations must use plain, non-technical language.
- NEVER make code changes without the user's explicit approval.
- NEVER make more than 3 consecutive fix attempts. If 3 attempts fail, escalate per the protocol below.

## Step 1 — Collect the symptom

Ask the user to describe:
- What did you do? (which button, which page, which action)
- What did you see? (error message, blank page, wrong data, nothing happened)
- What did you expect to see instead?
- Has this worked before? If so, what changed since then?

## Step 2 — Explain the root cause in plain language

Investigate the codebase to identify the likely root cause. Then explain it to the user in plain language:
- "The reason this happens is: [explanation without code jargon]"
- "This is similar to: [real-world analogy if helpful]"

Ask the user: "Does this explanation match what you're seeing?" Wait for confirmation.

If the user says the explanation does not match, re-investigate. Do NOT proceed to a fix with a mismatched diagnosis.

## Step 3 — Propose the smallest possible fix

- State which file(s) will be changed (by name, no more than 2 files)
- Describe what the change will do in plain language
- State what will NOT be affected by this change

Ask the user: "Should I go ahead with this fix?" Wait for approval.

## Step 4 — Apply and verify

- Make the approved change
- Provide specific verification steps the user can follow:
  - "Open [page/screen]"
  - "Click [button] / Enter [value]"
  - "You should see [expected result]"
  - "Also check: [secondary thing that should still work]"

Ask the user to perform the verification and report back.

## Step 5 — If verification fails

Track the attempt count. On each failed attempt:
- Explain why the previous fix did not work (in plain language)
- Propose a different, minimal fix
- Get user approval before changing anything

**After 3 failed attempts, STOP and do one of the following:**
1. Suggest the user open a new conversation and re-describe the problem from scratch
2. Offer to revert all changes back to the last known working state (last Git commit)
3. Recommend isolating the problem by temporarily disabling the broken feature

Never continue making speculative changes beyond the 3-attempt limit.

## Step 6 — Record the fix

After a successful fix:
- If a verification checklist exists for this feature in `docs/verification/`, update it with any new verification steps
- If the bug revealed a missing behavior contract entry, add it to the relevant file in `docs/contracts/`
- Suggest a Git commit to save the working state

Expected output:

- Root cause explained in plain language, confirmed by user
- Minimal fix applied with user approval
- Verification steps provided and confirmed passing
- Working state saved via Git commit
- Or: escalation action taken if 3 attempts failed
