---
name: performance-check
description: 'Use when the app feels slow, a page takes too long to load, or an action hangs. Identifies the bottleneck, explains it in plain language, and fixes it step by step. Keywords: vibe coding, performance, slow, lag, loading, speed, optimize, non-technical, bottleneck, fast.'
argument-hint: 'What is slow, e.g.: the customer list page takes forever to load, saving an invoice is slow, the whole app feels sluggish'
user-invocable: true
---

# Performance Check Skill

Find out why the app is slow, explain the cause in plain language, and fix it in small verified steps without breaking existing features.

## When to Use

- A page, action, or feature is noticeably slow
- The app was fast but has gotten slower as more data was added
- A specific operation (search, save, load list) hangs or times out
- Before deploying, as a preventive check

## When Not to Use

- The app is broken (not slow, just wrong) — use `Diagnose and Fix` prompt instead
- The user wants to add features — performance work comes after features work correctly

## Core Principle

Never guess. Measure first, then fix. A "fix" that does not address the measured bottleneck wastes time and may break things.

## Procedure

### Phase 1 — Understand the symptom

Ask the user:
- What specific thing is slow? (page load, button click, list display, search, save)
- How slow is it? (a few seconds, 10+ seconds, never finishes)
- Did it used to be faster? If so, what changed?
- How much data is there? (e.g., 10 customers vs 10,000 customers)

### Phase 2 — Measure, don't guess

Identify the type of slowness based on the symptom:

| Symptom | Likely cause | Where to look |
|---------|-------------|---------------|
| List loads slowly with lots of data | Missing database index or loading too many records | Database queries, pagination |
| Page loads slowly on first open | Loading too much data or too many files upfront | Initial data fetch, bundle size |
| Saving is slow | Synchronous operations blocking the response | Write operations, external calls |
| Search is slow | No index on searched fields | Database query plan |
| Everything is slow | No caching, repeated identical queries | Query patterns, caching layer |

Add timing measurements to identify where time is actually spent:
- Log timestamps around the slowest operation
- Check database query execution time (use EXPLAIN QUERY PLAN for SQLite, EXPLAIN ANALYZE for PostgreSQL)
- Check network request times in browser dev tools if frontend

Report the measurements before proposing any fix:
> "I measured the slow part. Here's what I found:
> - [Operation] takes [X]ms total
> - [Specific step] accounts for [Y]ms of that
> - The bottleneck is: [plain language explanation]"

### Phase 3 — Explain the cause in plain language

Use a real-world analogy when possible:
- "The database is scanning every single record to find matches, like searching for a name by reading every page of a phone book instead of using the alphabetical index."
- "The app is loading 5,000 customer records when it only shows 20 at a time, like printing the entire encyclopedia when you only need one page."

Ask: "Does this match what you're experiencing?" Wait for confirmation.

### Phase 4 — Fix with the minimal effective change

Common fixes by category (apply the simplest fix that addresses the measured bottleneck):

**Database query too slow:**
- Add an index on the queried fields
- Add pagination (load N records at a time instead of all at once)
- Select only needed columns instead of `SELECT *`

**Too much data loaded at once:**
- Implement pagination or infinite scroll
- Load details only when needed (lazy loading)
- Cache results that do not change often

**Repeated identical queries:**
- Cache the result in memory for a short time
- Batch multiple queries into one

**Frontend rendering slow:**
- Avoid re-rendering the whole list when only one item changes
- Virtualize long lists (only render visible items)

For each fix:
1. Announce what will change and why, in plain language
2. Change no more than 3 files per step
3. Measure again after the fix to confirm improvement
4. Report the before/after numbers:
   > "Before: [X]ms. After: [Y]ms. That's [N]x faster."

### Phase 5 — Verify no regression

After all fixes:
- Run existing tests if available
- Manually verify the affected feature still works correctly
- Run through the behavior contract items for the affected feature

### Phase 6 — Save progress

> "Performance fix complete. Ready to save? Commit: `Improve [feature] load time from Xms to Yms`"
