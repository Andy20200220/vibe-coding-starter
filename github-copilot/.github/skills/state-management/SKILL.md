---
name: state-management
description: 'Use when data shared across multiple pages or components becomes inconsistent, out of sync, or hard to track. Designs a clear data flow so changes in one place automatically update everywhere. Keywords: vibe coding, state, shared data, sync, multiple pages, component, data flow, non-technical, consistent.'
argument-hint: 'What data is getting out of sync, e.g.: the customer count in the header does not update after adding a customer, the selected item is lost when navigating between pages, the cart total is wrong'
user-invocable: true
---

# State Management Skill

Fix or design data flow so shared information stays consistent across the whole app. When one part of the app changes data, all other parts that show that data update automatically.

## When to Use

- Data shown in multiple places gets out of sync (e.g., a count in the header does not update when the list changes)
- Navigating between pages loses data the user just entered
- Two components show different versions of the same data
- The app needs to share login status, shopping cart, or user preferences across pages

## When Not to Use

- A single page has a local bug — use `Diagnose and Fix` prompt
- The feature has not been designed yet — define the behavior contract first

## Key Concepts (plain language)

- **State:** Data that the app remembers while it is running (like "which customer is selected" or "is the user logged in")
- **Global state:** Data that multiple pages or components need to share
- **Local state:** Data only one component needs (like "is this dropdown open")
- **Out of sync:** Two parts of the app show different values for the same piece of data

## Procedure

### Phase 1 — Map the problem

Ask the user to describe what is inconsistent:
- What data is getting out of sync?
- Where is it shown? (which pages, which components)
- When does it go wrong? (after saving, after navigating, after logging in)

Read the behavior contract and current source files to understand the existing data flow.

### Phase 2 — Categorize the state

For each piece of shared data, categorize:

| Category | Examples | Where to store |
|----------|---------|---------------|
| Auth/session | Is logged in, current user, permissions | Global store or context |
| App-wide data | Shopping cart, notification count, theme | Global store or context |
| Feature data | Current customer list, selected invoice | Feature-level store or context |
| Form data | What the user is typing right now | Local component state |
| UI state | Is modal open, which tab is active | Local component state |

Only elevate to global state if truly needed across multiple unrelated pages. Overusing global state creates its own problems.

### Phase 3 — Design the solution

Based on the tech stack (from `docs/design/tech-selection.md`), recommend the appropriate approach:

- **React:** useState for local, useContext or Zustand/Redux for global
- **Vue:** ref/reactive for local, Pinia or Vuex for global
- **Vanilla JS:** Module-level variables or a simple event bus
- **Next.js/Nuxt:** Server state with SWR/React Query for data fetching + Zustand for client state

Explain the approach in plain language:
> "Right now, every page loads the customer count separately. I'll create a central customer list that all pages share. When you add a customer on one page, the count in the header updates automatically because they are both reading from the same central list."

### Phase 4 — Implement

For each piece of state to migrate:
1. Announce what will change and why
2. Change no more than 3 files per step
3. Migrate one piece of state at a time
4. Verify after each step that the sync is working

Verification steps for sync:
> "1. Open [page A] and note the [value]
> 2. Go to [page B] and make a change
> 3. Go back to [page A] — the [value] should now show the updated number without refreshing"

### Phase 5 — Save

> "State management fixed. Commit: `Fix [data] sync across [pages/features]`"
