---
name: error-logging
description: 'Use to set up error logging so when something breaks in production, there is a record of what happened. Without logging, bugs in live apps are invisible. Keywords: vibe coding, logging, error tracking, production, debug, monitor, alert, non-technical, visibility.'
argument-hint: 'What to log, e.g.: set up basic error logging, track API errors, log when payments fail, set up error alerts'
user-invocable: true
---

# Error Logging Skill

Set up logging so errors in production leave a trail. When something breaks after the app is live, logs tell you exactly what went wrong so you can fix it quickly instead of guessing.

## When to Use

- Before deploying the app for the first time
- After a bug was discovered in production that was invisible without logs
- When the app starts behaving strangely and there is no way to find out why
- When critical actions (payments, data saves) need an audit trail

## When Not to Use

- Debugging a known bug in development — use `Diagnose and Fix` prompt instead
- The app has not been deployed yet and logging is not a priority

## Key Concepts (plain language)

- **Log:** A saved record of what happened (like a diary for the app)
- **Error log:** A record of when and why something went wrong
- **Log level:** How serious the event is (INFO = normal activity, WARN = something odd, ERROR = something broke)
- **Log destination:** Where logs are saved (console, a file, a cloud service)

## Procedure

### Phase 1 — Understand the context

Read `docs/design/tech-selection.md` to understand the tech stack and deployment environment.

Ask:
- Is this app already deployed, or being set up for first deployment?
- Are there specific actions where errors are most costly? (payments, data saves, auth)
- Does the user want simple file/console logging or a monitoring service?

For most small projects: start with structured console logging + a free service like Sentry or Logtail.

### Phase 2 — Define what to log

Categorize events to log:

**Always log (ERROR level):**
- Unhandled exceptions and crashes
- Database connection failures
- Failed external API calls (payment gateway, email service, etc.)
- Auth failures that look like attacks (many failed logins from the same IP)
- Data writes that failed silently

**Log when helpful (WARN level):**
- Slow database queries (over 1 second)
- Deprecated feature usage
- Unusual input values that were rejected by validation
- Cache misses on frequently accessed data

**Log for audit trail (INFO level):**
- User login / logout
- Record created / updated / deleted (with who did it and when)
- Payment transactions (amount, status, not card numbers)
- Admin actions

**Never log:**
- Passwords (in any form)
- Full credit card numbers
- Session tokens or API keys
- Full personal data beyond what is needed for the audit

### Phase 3 — Implement logging

**Step 1: Add a logging utility**
Create a single logging module the whole app uses. This ensures consistent format and makes it easy to change the logging destination later.

Log format should include:
- Timestamp
- Level (ERROR/WARN/INFO)
- Message
- Context (which user, which record ID, which action)
- For errors: the error message and stack trace

**Step 2: Add error logging to critical paths**
Wrap these in try/catch with logging:
- Database operations
- External API calls
- File operations
- Auth operations

**Step 3: Add a global error handler**
Catch unhandled errors at the top level so nothing crashes silently.

**Step 4: (Optional) Connect to a monitoring service**
For production apps, connect to a free-tier service:
- **Sentry** — catches and reports errors automatically, free tier available
- **Logtail / BetterStack** — log aggregation and search, free tier available

Explain to the user:
> "Sentry works like a security camera for your app. When something breaks, it automatically records exactly what happened, who was affected, and sends you an alert. You can set it up in about 10 minutes."

### Phase 4 — Test the logging

Intentionally trigger an error (in a test environment) and verify:
- The error is captured
- The log entry contains enough information to understand what went wrong
- Sensitive data is NOT in the log
- The user-facing error message is friendly, not a raw stack trace

### Phase 5 — Document alert thresholds

If using a monitoring service, set up alerts for:
- Any ERROR-level event → immediate notification
- More than 10 WARN events per minute → notification
- Zero activity for an extended period (app may be down) → notification

### Phase 6 — Save

> "Error logging set up. Commit: `Add error logging and monitoring`"

Update `docs/design/tech-selection.md` to record what logging solution is in use.
