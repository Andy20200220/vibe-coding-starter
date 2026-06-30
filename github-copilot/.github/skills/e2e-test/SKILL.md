---
name: e2e-test
description: 'Use when you want to verify complete user journeys work end-to-end by simulating real user interactions: clicking buttons, filling forms, navigating pages. Catches bugs that unit tests miss because they test the whole system together. Keywords: vibe coding, e2e, end-to-end, integration test, user journey, simulate, browser, Playwright, Cypress, non-technical.'
argument-hint: 'What user journey to test, e.g.: test the full login flow, test creating and saving a customer, test the complete invoice creation process'
user-invocable: true
---

# E2E Test Skill

Write and run end-to-end tests that simulate a real user clicking through the app. These tests catch bugs that unit tests miss because they test the whole system working together — frontend, backend, and database.

## When to Use

- A critical user journey (login, checkout, data creation) needs automated verification
- The app has enough features that manually testing everything after each change takes too long
- A bug was found in production that unit tests did not catch (because it only appeared when multiple parts worked together)
- Before a major deployment, to verify all critical paths still work

## When Not to Use

- The feature is not yet implemented — implement first
- Unit tests are not yet in place — add unit tests first with `test-generation` skill, then E2E
- You just want to test one function in isolation — use `test-generation` skill instead

## Key Concepts (plain language)

- **E2E test:** A test that opens a real browser, clicks buttons, fills in forms, and checks what appears on screen — just like a real user would
- **Test scenario:** One complete user journey from start to finish (e.g., "user logs in, creates a customer, and sees them in the list")
- **Assertion:** A check that confirms something is true (e.g., "the page shows 'Customer saved'")
- **Headless:** Running the browser invisibly in the background (faster, used in automated runs)

## Procedure

### Phase 1 — Choose the E2E framework

Read `docs/design/tech-selection.md` for the tech stack. Recommend based on stack:

| Stack | Recommended tool | Why |
|-------|-----------------|-----|
| Any web app | **Playwright** | Modern, fast, works with all browsers, good for most projects |
| React/Next.js | Playwright or Cypress | Both work well |
| Vue/Nuxt | Playwright | Best support |
| Simple local app | Playwright | Easiest setup |

If no E2E framework is installed:
> "To test like a real user would, I recommend Playwright. It automatically opens a browser, clicks through your app, and reports if anything breaks. Should I set it up? It takes about 5 minutes."

Wait for approval before installing.

### Phase 2 — Identify the journeys to test

Read the behavior contracts in `docs/contracts/`. Identify the most critical user journeys — usually:
1. The primary action (the main thing the app does)
2. The auth flow (login/logout if the app has it)
3. Any flow involving data that cannot be easily recovered (delete, payment, send)

For each journey, write a plain-language description:
> "Journey 1: User logs in with valid credentials → sees the dashboard → creates a new customer → customer appears in the list"

Present the list and ask: "Which of these should I test first? I recommend starting with [most critical one]."

### Phase 3 — Write the tests

For each journey, write a test that:
1. Sets up any required starting state (e.g., a test user account exists)
2. Opens the app in a browser
3. Performs each step of the journey (click, type, navigate)
4. After each significant action, asserts the expected result is visible
5. Cleans up test data after the test runs (so tests do not leave residue)

**Test structure:**
```
Test: [Plain language journey name]

Setup: [what needs to exist before the test]

Steps:
1. Open [URL]
2. [Action] → Assert: [what should be visible]
3. [Action] → Assert: [what should be visible]
...

Teardown: [what to clean up after]
```

**Writing rules:**
- Use stable selectors (by text content, ARIA role, or data-testid — not CSS classes that change)
- Add meaningful test names: `"user can create a customer and see them in the list"` not `"test_1"`
- Each test must be independent (not depend on another test having run first)
- Tests must work in any order

### Phase 4 — Run the tests

Run the test suite and report results:
- All tests pass → show summary
- A test fails → explain in plain language what failed and why:
  > "The test for [journey] failed at step [N]. It expected to see '[expected text]' on the page, but saw '[actual text]' instead. This means [plain language explanation]."

If a test reveals a real bug (not a test mistake), switch to `Diagnose and Fix` protocol.

### Phase 5 — Add to the project workflow

Document how to run E2E tests:
> "To run all E2E tests: type `[command]` in the terminal. The browser will open automatically and you will see the results. All tests should pass in about [time]."

Update `docs/verification/` to include:
> "Automated E2E tests: run `[command]` — all journeys should pass"

Suggest adding E2E tests to a pre-deployment checklist.

### Phase 6 — Save

> "E2E tests added for [journeys]. Commit: `Add E2E tests for [feature/journey names]`"
