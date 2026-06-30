---
name: deployment-check
description: 'Use before publishing or sharing a project to make sure it is ready: environment variables are set, secrets are not exposed, dependencies are correct, and the app works in the target environment. Keywords: vibe coding, deploy, publish, go live, release, checklist, production, environment, non-technical, pre-launch.'
argument-hint: 'Where you are deploying to, e.g.: share with a friend, put on Vercel, run on another computer, publish to the internet'
user-invocable: true
---

# Deployment Check Skill

Run a pre-deployment checklist to make sure the project is safe and ready to share or publish. Reports issues in plain language before anything is deployed.

## When to Use

- Before sharing the project with anyone
- Before putting the project on a hosting service (Vercel, Netlify, Railway, etc.)
- Before running the project on a different computer
- When something works locally but fails on another machine

## When Not to Use

- The project is still in development and not ready for sharing
- A specific error has occurred after deployment — use `Diagnose and Fix` prompt instead

## Procedure

### Phase 1 — Understand the target

Ask if not specified:
> "Where are you deploying this?
> (a) Sharing with someone to run on their computer
> (b) Hosting online (Vercel, Netlify, Railway, Heroku, etc.)
> (c) Running on your own server
> (d) Just making sure it works before a demo"

Read `docs/design/tech-selection.md` to understand the tech stack and deployment context.

### Phase 2 — Run the checklist

Check each category below. Rate each issue: 🔴 Blocker / 🟡 Risk / 🟢 Minor.

**A. Secrets and credentials — nothing sensitive exposed**
- Are there hardcoded API keys, passwords, or tokens in source files?
- Is there a `.env` file with secrets? Is it listed in `.gitignore`?
- Does the project read secrets from environment variables, not hardcoded values?
- Are there any `console.log` statements printing sensitive data?

**B. Environment configuration — works outside your computer**
- Are all required environment variables documented? (e.g., in a `.env.example` file)
- Are there hardcoded file paths that only exist on your machine?
- Are there hardcoded `localhost` URLs that need to change for production?
- Does the app connect to the right database/service in production vs. development?

**C. Dependencies — complete and correct**
- Does `package.json` (or equivalent) list all required dependencies?
- Are there packages installed locally but not in the dependency file?
- Are dependency versions pinned or are they using loose ranges that could break?

**D. Build and start — runs cleanly from scratch**
- Does `npm install` (or equivalent) complete without errors?
- Does the build command succeed?
- Does the app start without errors on a fresh setup?
- Is the start command documented somewhere (README or deployment guide)?

**E. Data and storage — data persists correctly**
- Does the database initialize correctly on a fresh setup?
- Are file storage paths relative (not hardcoded to your machine)?
- Are required database migration files included?

**F. Error handling — failures are handled gracefully**
- Do API errors show a user-friendly message instead of crashing?
- Is there a fallback if an external service is unavailable?

**G. Final feature check**
- Run through the verification checklists in `docs/verification/` for all MVP features
- Confirm all features work as specified in their behavior contracts

### Phase 3 — Report

---
## Deployment Readiness Report

Target: [deployment target]
Date: [today]

### Overall Status
🔴 Not ready / 🟡 Ready with caveats / ✅ Ready to deploy

### Issues Found

#### 🔴 Blockers — fix before deploying
| # | Issue | How to fix |
|---|-------|-----------|
| 1 | ... | ... |

#### 🟡 Risks — strongly recommended to fix
| # | Issue | How to fix |
|---|-------|-----------|

#### 🟢 Minor — optional improvements
| # | Issue | How to fix |
|---|-------|-----------|

### Deployment Steps
[Step-by-step instructions specific to the target platform, in plain language]

---

### Phase 4 — Fix and deploy

> "Would you like me to fix the blockers now? I'll handle them one at a time."

After all blockers are resolved, provide the exact deployment steps for the target platform.
