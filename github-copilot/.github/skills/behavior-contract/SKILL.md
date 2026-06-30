---
name: behavior-contract
description: 'Use when a non-technical user has confirmed a product definition or feature description and needs it converted into a precise, item-by-item behavior contract that constrains AI implementation. Use after requirement clarification, before any code is written. Keywords: vibe coding, behavior contract, feature spec, acceptance criteria, non-technical, user action, system response, validation.'
argument-hint: 'Feature or product definition to convert, e.g.: the login feature from product-definition.md, the invoice management feature'
user-invocable: true
---

# Behavior Contract Skill

Convert a confirmed product definition or feature description into a precise behavior contract that a non-technical user can review item by item.

## When to Use

- A feature idea has been discussed and the user has confirmed the general direction
- The next step is to define exactly what the system should do for each user action before writing code
- The user does not know code and needs plain-language specifications

## When Not to Use

- The idea is still vague — use `Clarify My Requirement` prompt first
- A behavior contract already exists and code is ready — use `guided-implementation` skill
- A bug needs fixing — use `Diagnose and Fix` prompt

## Procedure

1. **Read existing context.** Check `docs/contracts/` for any existing product definition or behavior contracts. Check `docs/design/` for any technical design. Do not duplicate what already exists.

2. **Identify the feature scope.** Confirm with the user which feature is being specified. One behavior contract covers exactly one feature. Do not combine multiple features.

3. **List all user actions.** For the target feature, enumerate every action a user can take:
   - Button clicks
   - Form submissions
   - Navigation actions
   - Data entry
   - Selections, toggles, filters

4. **Define the system response for each action.** For every user action, state:
   - **Normal case:** What the user should see when the action succeeds
   - **Validation errors:** What happens when the input is invalid (empty, wrong format, too long, etc.)
   - **Business rule violations:** What happens when the action conflicts with a rule (duplicate, unauthorized, limit exceeded, etc.)
   - **Edge cases:** What happens with boundary values (zero, maximum, first time, already exists, etc.)

5. **Define verification steps.** For each behavior item, write a concrete verification step the user can perform:
   - "Open [page]"
   - "Enter [specific value]"
   - "Click [button]"
   - "You should see [expected result]"

6. **Review with the user.** Present the behavior contract item by item. For each item:
   - Ask the user: "Is this correct?"
   - If the user is unsure, mark the item as "TBD — needs decision" and move on
   - If the user disagrees, revise immediately

7. **Save the contract.** Save the confirmed behavior contract to `docs/contracts/<feature-name>.md` using this structure:

```markdown
# Behavior Contract: [Feature Name]

Status: Confirmed / Partially confirmed (N items TBD)
Date: YYYY-MM-DD

## User Actions and System Responses

### [Action Group Name]

| # | User Action | Normal Response | Error / Edge Cases | Verification |
|---|------------|----------------|-------------------|--------------|
| 1 | ... | ... | ... | ... |

## TBD Items

- Item N: [description of what needs to be decided]

## Notes

- [Any assumptions or constraints discussed with the user]
```

8. **State next step.** After saving:
   - If all items are confirmed → next step is `guided-implementation` skill
   - If TBD items remain → resolve them before implementation
   - If the feature depends on another un-built feature → note the dependency

## Output Guidance

- All language must be non-technical. No code, no technical jargon, no framework references.
- One contract per feature. Do not merge multiple features.
- Every behavior item must have a corresponding verification step.
- Do not invent behaviors the user did not confirm. Mark unknowns as TBD.
