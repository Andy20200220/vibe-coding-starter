---
name: auth-design
description: 'Use when a feature requires user login, registration, or access control. Designs the authentication and authorization system before writing any code, covering login flows, session management, and role-based permissions. Keywords: vibe coding, auth, login, register, permissions, roles, session, token, security, non-technical.'
argument-hint: 'What auth is needed, e.g.: users need to log in, some pages should only be visible to admins, users should only see their own data'
user-invocable: true
---

# Auth Design Skill

Design login, registration, and permission systems before writing code. Covers user flows, session handling, role-based access control, and security requirements.

## When to Use

- A feature requires users to log in before accessing it
- Different users should see or do different things (admin vs. regular user)
- Users should only be able to access their own data, not other users' data
- Registration or account management features are needed

## When Not to Use

- Auth is already working and you just need to add a feature — use `behavior-contract` skill
- An auth bug exists — use `Diagnose and Fix` prompt

## Critical Security Principles

These are non-negotiable. Any auth implementation that violates them must be fixed before shipping:
1. **Never store plain-text passwords** — always hash with bcrypt or equivalent
2. **Never put auth logic only on the frontend** — always verify on the server
3. **Use HTTPS in production** — never send credentials over plain HTTP
4. **Tokens must expire** — sessions and JWTs must have expiry times
5. **Rate-limit login attempts** — prevent brute-force attacks

## Procedure

### Phase 1 — Understand the requirements

Ask the user:
- Who are the different types of users? (e.g., regular users, admins, guests)
- What can each type of user do? What are they NOT allowed to do?
- Should users register themselves, or does an admin create accounts?
- How do users log in? (email + password, phone + OTP, social login like Google)
- How long should a login session last? (until they log out, 24 hours, 30 days)
- What happens when a session expires? (redirected to login, asked to re-authenticate)

### Phase 2 — Design the auth flows

Document each flow in plain language:

**Registration flow:**
- What information does the user provide?
- What validation runs on each field?
- Is email/phone verification required?
- What happens immediately after successful registration?

**Login flow:**
- What credentials does the user enter?
- How many failed attempts before lockout?
- How long is the lockout?
- Is "remember me" supported? What does it do technically?

**Session management:**
- Where is the session stored? (cookie, localStorage, server-side)
- What is the session expiry?
- What happens on logout? (clear session, redirect)

**Password reset flow:**
- How does the user request a reset?
- How is the reset link/code delivered?
- How long is the reset link valid?
- Can the same reset link be used twice?

### Phase 3 — Design permissions (if multiple roles)

Create a permission matrix:

| Action | Guest | Regular User | Admin |
|--------|-------|-------------|-------|
| View public pages | ✅ | ✅ | ✅ |
| View own data | ❌ | ✅ | ✅ |
| View all users' data | ❌ | ❌ | ✅ |
| Create records | ❌ | ✅ | ✅ |
| Delete any record | ❌ | ❌ | ✅ |

For each protected resource, define:
- Who can read it?
- Who can create it?
- Who can edit it?
- Who can delete it?

### Phase 4 — Security checklist

Before implementation, confirm:
- [ ] Passwords will be hashed (bcrypt, Argon2, or equivalent)
- [ ] Login attempts will be rate-limited
- [ ] Sessions will have expiry
- [ ] Protected routes verified on server, not just frontend
- [ ] Sensitive actions require re-authentication (e.g., changing password, deleting account)
- [ ] No sensitive data in URLs or logs

### Phase 5 — Save the design

Save to `docs/design/auth-design.md`:
- User roles and permissions matrix
- Each flow documented
- Security decisions and reasoning

### Phase 6 — Next step

> "Auth design is saved. Next step: use `behavior-contract` skill to write the detailed behavior contracts for each auth feature (login, register, password reset), then implement with `guided-implementation`."
