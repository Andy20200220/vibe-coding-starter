---
name: test-generation
description: 'Use when a feature has been implemented and verified manually, and the user wants automated tests so future changes do not silently break it. Generates tests that match the behavior contract and explains what each test checks. Keywords: vibe coding, test, automated test, regression, verify, non-technical, test generation, quality assurance.'
argument-hint: 'Feature to generate tests for, e.g.: the login feature, the invoice save button, all features in docs/contracts/'
user-invocable: true
---

# Test Generation Skill

Generate automated tests for an implemented feature based on its behavior contract. Tests act as a permanent verification net — if future changes break something, the tests catch it automatically.

## When to Use

- A feature is fully implemented and manually verified
- The user wants protection against future changes breaking existing features
- The project has enough features that manually re-checking everything after each change is slow

## When Not to Use

- The feature is not yet implemented — implement first
- No behavior contract exists — create the contract first, then generate tests
- The user is reporting a current bug — fix the bug first, then add tests to prevent recurrence

## Procedure

### Phase 1 — Read the contract

Read the behavior contract from `docs/contracts/<feature-name>.md`. List every behavior item that will be tested.

Read `docs/design/tech-selection.md` to understand the testing tools available for this tech stack. If no test framework is set up, note this.

### Phase 2 — Check test infrastructure

Check whether a test framework is already installed and configured:
- Is there a test script in `package.json` (or equivalent)?
- Is there an existing test directory (`tests/`, `__tests__/`, `spec/`)?
- Are there existing test files to follow as a pattern?

If no test framework exists:
> "This project doesn't have a test framework set up yet. For [tech stack], I recommend [framework] because [plain language reason]. Should I set it up first? It takes about [N] minutes."

Wait for approval before installing anything.

### Phase 3 — Write the tests

For each behavior in the contract, write one or more tests:

- **Normal case test:** Does the happy path work?
- **Validation error test:** Does invalid input produce the right error message?
- **Edge case test:** Does boundary input (empty, zero, max length) behave correctly?
- **Business rule test:** Does a rule violation (duplicate, unauthorized) produce the right response?

Test-writing rules:
- Each test must map to a specific behavior contract item (add a comment linking them)
- Test names must be readable plain English: `"shows error message when phone number is empty"` not `"test_validation_01"`
- Tests should reflect what the USER sees, not internal implementation details
- No more than one behavior per test (keep them small and specific)

### Phase 4 — Explain the tests

After writing tests, show the user a plain-language summary:

> "I've written [N] tests for the [feature] feature:
>
> ✅ [Test name] — checks that [plain language explanation]
> ✅ [Test name] — checks that [plain language explanation]
> ...
>
> These tests will run automatically whenever you want to check that everything still works.
> To run them, [specific command, e.g.: 'type `npm test` in the terminal']."

### Phase 5 — Run the tests

Run the tests immediately after writing them:
- All tests should pass (the feature is already implemented)
- If any test fails, it means either the test is wrong or the implementation has a gap

Report the results:
> "All [N] tests passed ✓" or
> "[N] tests passed, [M] failed. The failures are: [plain language explanation of what each failure means]"

If failures exist, fix either the test (if the test is wrong) or the implementation (if there is a real gap) before considering the work done.

### Phase 6 — Update verification checklist

Update `docs/verification/<feature-name>.md` to include:
> "Automated tests: run `[command]` — all tests should pass"
