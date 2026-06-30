---
name: project-health-check
description: 'Use when a non-technical user wants to understand the current state of their project: what is working, what has risks, what is incomplete, and whether the codebase is healthy. Produces a plain-language report. Keywords: vibe coding, health check, project state, code review, risk, incomplete, technical debt, non-technical, status, audit.'
argument-hint: 'Optional focus area, e.g.: check everything, focus on the login feature, check before adding a new feature'
user-invocable: true
---

# Project Health Check Skill

Scan the project and produce a plain-language health report covering what is working, what has risks, what is incomplete, and what to fix before adding more features.

## When to Use

- Before starting a new feature (is the project in a good state to add to?)
- After a period of bug fixing (did the fixes introduce new issues?)
- When the project feels slow or fragile
- When the user wants a status overview without reading code

## When Not to Use

- The project has not been initialized yet — nothing to check
- A specific bug is reported — use `Diagnose and Fix` prompt instead
- The user wants to add a feature — use `behavior-contract` skill instead

## Procedure

### Phase 1 — Collect context

1. Read `docs/contracts/` — list all behavior contracts and their status (confirmed / TBD items remaining)
2. Read `docs/verification/` — list all verification checklists
3. Read `docs/design/tech-selection.md` if it exists
4. Read `docs/contracts/product-definition.md` if it exists
5. Scan the project source code structure (file names and directory layout — do not read every file in full)
6. Check for obvious signals: very large files, files named "temp" or "test" or "old", TODO/FIXME comments, empty error handlers

### Phase 2 — Run the health checks

Check each of the following areas. For each issue found, note its severity: 🔴 Blocker / 🟡 Risk / 🟢 Minor.

**A. Contract coverage**
- How many features have a confirmed behavior contract?
- How many features have been implemented without a contract?
- Are there TBD items in any contract that have not been resolved?

**B. Verification coverage**
- How many implemented features have a verification checklist in `docs/verification/`?
- Are there features with no way to verify they still work?

**C. Code quality signals**
- Are there files that seem very large (a sign that one file is doing too many things)?
- Are there TODO or FIXME comments left in the code?
- Are there files or functions with names that suggest they are temporary?
- Are there empty catch blocks or error handlers that silently swallow errors?

**D. Dependency health** (if package.json or similar exists)
- Are there dependencies that have known issues or are very outdated?
- Are there unused dependencies?

**E. Data safety**
- Are there hardcoded passwords, API keys, or secrets visible in source files?
- Is user data handled in ways that seem risky?

**F. Feature completeness**
- Compare implemented features against `docs/contracts/product-definition.md`
- Which MVP features are fully implemented and verified?
- Which are partially done?
- Which have not been started?

### Phase 3 — Produce the report

Write the report in plain language. No code snippets unless absolutely necessary. Use the following structure:

---
## Project Health Report

Date: [today]

### Overall Status
[One sentence: Green / Yellow / Red and why]

### What Is Working Well
- [Item]
- [Item]

### Issues Found

#### 🔴 Blockers — Fix before adding new features
| # | Issue | Why it matters | Suggested action |
|---|-------|---------------|-----------------|
| 1 | ... | ... | ... |

#### 🟡 Risks — Should fix soon
| # | Issue | Why it matters | Suggested action |
|---|-------|---------------|-----------------|
| 1 | ... | ... | ... |

#### 🟢 Minor — Fix when convenient
| # | Issue | Why it matters | Suggested action |
|---|-------|---------------|-----------------|
| 1 | ... | ... | ... |

### Feature Status
| Feature | Contract | Verified | Status |
|---------|----------|----------|--------|
| [name] | ✅ / ❌ | ✅ / ❌ | Done / Partial / Not started |

### Recommended Next Action
[One clear recommendation: what to do first, in plain language]

---

### Phase 4 — Offer to fix

After presenting the report, ask:
> "Would you like me to fix any of the blockers or risks now? I can take them one at a time."

If the user says yes, address one issue at a time using the appropriate skill or prompt (`guided-implementation` for missing features, `Diagnose and Fix` for bugs, `behavior-contract` for missing contracts).
