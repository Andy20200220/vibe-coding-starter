---
name: user-flow-design
description: 'Use when a feature involves multiple steps or screens and the path through them needs to be designed before coding. Reduces user errors and re-work from poorly thought-out navigation. Keywords: vibe coding, user flow, multi-step, navigation, journey, screen flow, UX, design, non-technical.'
argument-hint: 'What flow to design, e.g.: the onboarding flow for new users, the checkout process, the multi-step form for creating an invoice, the flow for resetting a password'
user-invocable: true
---

# User Flow Design Skill

Design the step-by-step path users take through a multi-screen feature before writing any code. A clear flow prevents rework from confusing navigation or missing states.

## When to Use

- A feature involves more than one screen or step
- Users keep getting confused or lost in an existing flow
- Designing onboarding, checkout, registration, or any multi-step process
- Before implementing a feature that involves conditional navigation (if X then go to screen A, else screen B)

## When Not to Use

- The feature is a single screen with no navigation — use `behavior-contract` skill instead
- A user is already stuck in a broken flow — use `Diagnose and Fix` prompt first

## Key Concepts (plain language)

- **User flow:** The sequence of screens a user goes through to complete a task
- **Happy path:** The ideal path when everything goes right
- **Error path:** What happens when something goes wrong (invalid input, network failure, etc.)
- **Dead end:** A state where the user cannot proceed and does not know what to do
- **Back navigation:** Can the user go back? Will they lose their progress?

## Procedure

### Phase 1 — Understand the task

Ask the user:
- What is the user trying to accomplish? (the goal of this flow)
- What does the user need to have/know before starting? (prerequisites)
- What does success look like? (what the user sees when done)
- What are the main ways it could go wrong?

### Phase 2 — Map the happy path

List every screen/step in the ideal flow:

```
Step 1: [Screen name] — User does [action]
    ↓
Step 2: [Screen name] — User sees [content], does [action]
    ↓
Step 3: [Screen name] — User confirms [something]
    ↓
Success: [What the user sees / where they land]
```

Keep it in plain language — no wireframes needed, just the narrative.

### Phase 3 — Map all branches and error paths

For each step, ask:
- What if the user goes back? (is that allowed? does data persist?)
- What if the user's input is invalid? (stay on screen, show error, let them retry)
- What if a required step is skipped? (can they skip? what happens?)
- What if a network request fails? (retry option? fallback?)
- What if the user is not authorized? (redirect to login? show error?)
- What if the user abandons midway? (is progress saved? can they resume?)

Document each branch:
```
Step 2 — if phone number is invalid:
  → Stay on Step 2, show error under the field, let user correct and retry

Step 3 — if verification code is expired:
  → Show "Code expired" message with "Resend code" button, return to Step 2
```

### Phase 4 — Check for dead ends

A dead end is any state where:
- The user cannot proceed (error with no resolution)
- The user cannot go back
- The user does not know what to do next

For each identified dead end:
> "At [step], if [condition], the user would be stuck. I recommend: [solution — add a retry button / add a back link / add a help message]"

### Phase 5 — Review with the user

Present the full flow narrative:
> "Here is how the [feature] will work:
>
> 1. The user starts at [screen]. They [action].
> 2. They see [screen]. If their [input] is valid, they continue. If not, [what happens].
> 3. ...
> 4. When done, they see [success state] and are taken to [destination].
>
> If they abandon at step [N], [what happens to their progress].
> If [error scenario], [what they see and can do].
>
> Does this match what you expected?"

Revise until confirmed.

### Phase 6 — Save the flow

Save to `docs/design/user-flows.md`:

```markdown
# User Flow: [Feature Name]

## Goal
[What the user is trying to accomplish]

## Prerequisites
[What the user needs before starting]

## Steps

### Step 1: [Screen Name]
- User sees: [description]
- User does: [action]
- Next: Step 2

### Step 2: [Screen Name]
- User sees: [description]
- User does: [action]
- If valid: proceed to Step 3
- If invalid: [error state description]
- Next: Step 3

[...]

## Success State
[What the user sees and where they land]

## Error States
[Table of error conditions and resolutions]

## Abandonment
[What happens to partial progress]
```

### Phase 7 — Next step

> "User flow is documented. Next step: create behavior contracts for each screen in this flow using the `behavior-contract` skill."
