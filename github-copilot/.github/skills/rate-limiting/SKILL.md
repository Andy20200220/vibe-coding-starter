---
name: rate-limiting
description: 'Use to protect API endpoints from being called too many times too quickly. Prevents abuse, brute-force attacks, and accidental overload. Keywords: vibe coding, rate limit, throttle, abuse, brute force, security, API protection, too many requests, non-technical.'
argument-hint: 'What to protect, e.g.: the login endpoint, all API endpoints, the send verification code button, the search feature'
user-invocable: true
---

# Rate Limiting Skill

Add limits to how many times an endpoint or action can be called in a given time window. Protects against brute-force attacks, spam, and accidental overload.

## When to Use

- The login or registration endpoint has no protection against repeated attempts
- A "send verification code" button could be clicked hundreds of times
- An API endpoint could be called in a loop by accident or maliciously
- Before deploying to production as a basic security hardening step

## When Not to Use

- The app is purely local with no server (no network requests to protect)
- Rate limiting is already in place — use `deployment-check` to verify it is configured correctly

## Key Concepts (plain language)

- **Rate limit:** A rule like "maximum 5 login attempts per minute per IP address"
- **Throttling:** Slowing down requests instead of blocking them outright
- **429 Too Many Requests:** The standard HTTP response when a rate limit is exceeded
- **IP-based limiting:** Limits applied per visitor's IP address
- **User-based limiting:** Limits applied per logged-in user account

## Procedure

### Phase 1 — Identify endpoints to protect

Categorize all API endpoints by risk:

| Risk | Endpoint type | Recommended limit |
|------|--------------|-------------------|
| Critical | Login, password reset, OTP send | 5 attempts / 15 minutes / IP |
| High | Registration, payment, account changes | 10 requests / hour / IP |
| Medium | Search, data creation | 60 requests / minute / user |
| Low | Read-only data, public pages | 300 requests / minute / IP |

Ask the user to confirm any business-specific limits (e.g., "each user can send 3 invoices per day").

### Phase 2 — Choose the implementation approach

Based on the tech stack from `docs/design/tech-selection.md`:

**Node.js / Express:**
- `express-rate-limit` package — simple, works for most cases
- Redis-backed rate limiting for multi-server deployments

**Python / FastAPI or Flask:**
- `slowapi` (FastAPI) or `Flask-Limiter` (Flask)

**Next.js:**
- Middleware-based rate limiting using `@upstash/ratelimit` (for edge/serverless)
- Or `express-rate-limit` with a custom server

**At infrastructure level (recommended for production):**
- Nginx or Cloudflare rate limiting — protects before requests reach the app

Recommend the simplest option that fits the project's scale.

### Phase 3 — Implement

For each endpoint group:

1. Install the rate limiting package (if needed)
2. Configure the rule: `max requests, window duration, key (IP or user ID)`
3. Set the response for exceeded limits:
   - HTTP 429 status
   - User-facing message: "Too many attempts. Please wait [time] before trying again."
   - Include `Retry-After` header with seconds until the limit resets
4. Add to the endpoint

Change no more than 3 files per step.

### Phase 4 — Handle the user experience for exceeded limits

The error message shown to users must be:
- Clear: tell them what happened and how long to wait
- Not revealing: do not disclose how many attempts are allowed (helps attackers)
- Consistent: same message format as other app errors

Good: "Too many login attempts. Please wait 15 minutes before trying again."
Bad: "Rate limit exceeded: 5/5 attempts used. Resets in 847 seconds."

Update the behavior contract if it does not yet document what users see when rate-limited.

### Phase 5 — Test the limits

For each configured limit, verify:
1. Normal use works (under the limit, no errors)
2. Exceeding the limit returns 429 with the correct message
3. After the window resets, requests succeed again

> "To test: [specific steps to trigger the rate limit and verify the response]"

### Phase 6 — Save

> "Rate limiting added. Commit: `Add rate limiting to [endpoints]`"

Document the limits in `docs/design/tech-selection.md` under a Security section.
