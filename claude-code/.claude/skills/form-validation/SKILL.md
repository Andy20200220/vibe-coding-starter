---
name: form-validation
description: 'Use when a form needs input validation: required fields, format checking, error messages, and preventing bad data from being submitted. Ensures consistent validation on both frontend and backend. Keywords: vibe coding, form, validation, required, error message, input, format, submit, non-technical.'
argument-hint: 'Which form to validate, e.g.: the add customer form, the login form, the invoice creation form'
user-invocable: true
---

# Form Validation Skill

Add complete input validation to a form: required field checks, format validation, clear error messages, and backend verification. Prevents bad data and guides users to correct mistakes.

## When to Use

- A form can be submitted with empty or invalid data
- Error messages are missing, unclear, or inconsistent
- The same validation needs to happen on both the screen and the server
- A behavior contract specifies validation rules that are not yet implemented

## When Not to Use

- The form already has validation and it is working — use `code-review` for a plain-language quality review, or `requesting-code-review` if you need a formal quality gate
- The issue is a form submission bug, not missing validation — use `Diagnose and Fix`

## Key Principle

Validate in two places: (1) on the screen before submitting (instant feedback), and (2) on the server after submitting (security). Never trust only frontend validation.

## Procedure

### Phase 1 — List all validation rules

Read the behavior contract in `docs/contracts/` for the feature. Extract all validation rules:

For each field, determine:
- Is it required?
- What format must it be? (email, phone, number, date, etc.)
- What are the length limits? (min/max characters)
- Are there business rules? (must be unique, must be in a valid range, must match another field)
- What is the exact error message for each failure?

If validation rules are missing from the behavior contract, ask the user:
> "For the [field] field: what should happen if the user leaves it empty? What if they enter the wrong format?"

Update the behavior contract with confirmed rules before writing code.

### Phase 2 — Design the error display

Confirm with the user how errors should appear:
- Inline under each field (recommended)
- Summary at the top of the form
- Both

Confirm the timing:
- On submit only (simpler)
- As the user types (more responsive, more complex)

Recommend inline + on-submit for most cases.

### Phase 3 — Implement frontend validation

For each field with validation rules:
1. Add the validation check
2. Show the specific error message under the field when invalid
3. Disable or block the submit button while there are errors (optional but recommended)
4. Clear the error when the user corrects the input

Implementation rules:
- Error messages must match exactly what is in the behavior contract
- Do not use generic messages like "Invalid input" — be specific: "Phone number must be 11 digits"
- Show one error at a time per field (the most important error first)

### Phase 4 — Implement backend validation

Add the same validation rules to the server-side handler:
- Return specific error codes for each validation failure
- Return which field failed, not just "validation error"
- Never trust frontend-only validation for security-sensitive fields

### Phase 5 — Test each validation rule

For each rule, provide a verification step:
> "To test the phone field:
> 1. Leave it empty and click Submit — you should see 'Phone number is required'
> 2. Enter 'abc' and click Submit — you should see 'Phone number must contain only digits'
> 3. Enter a valid phone number — the error should disappear"

Walk through every rule in the behavior contract.

### Phase 6 — Save

> "Form validation complete. Commit: `Add validation to [form name]`"

Also update the behavior contract if any rules were clarified during this process.
