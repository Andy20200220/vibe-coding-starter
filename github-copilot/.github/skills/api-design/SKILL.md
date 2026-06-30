---
name: api-design
description: 'Use when a feature requires communication between frontend and backend, or between this app and an external service. Designs the API contract before any code is written, preventing repeated interface changes. Keywords: vibe coding, API, interface, endpoint, request, response, frontend, backend, contract, design, non-technical.'
argument-hint: 'What interaction needs an API, e.g.: the save customer button needs to call the backend, the invoice list needs to load from the server, integrate with a payment service'
user-invocable: true
---

# API Design Skill

Design the interface between frontend and backend (or between this app and an external service) before writing any code. A clear API contract prevents wasted work from repeated interface changes.

## When to Use

- A feature requires the frontend to fetch or send data to the backend
- A new backend endpoint needs to be created
- This app needs to talk to a third-party service (payment, email, maps, etc.)
- The frontend and backend are being built separately or by different people

## When Not to Use

- The feature is entirely frontend-only (no data exchange with server)
- An API already exists and just needs to be called correctly — use `guided-implementation` skill
- Debugging a broken API call — use `Diagnose and Fix` prompt

## Procedure

### Phase 1 — Understand the interaction

Ask the user to describe the feature in plain language:
- What does the user do? (click Save, open a page, submit a form)
- What data needs to go to the server? (customer name, invoice amount, etc.)
- What does the server need to send back? (confirmation, a list of records, etc.)
- What should happen if the server is unavailable or the request fails?

Read the relevant behavior contract from `docs/contracts/` to understand all the cases the API must handle.

### Phase 2 — Design the API contract

For each interaction, define:

**Request:**
- Method: GET (read data) / POST (create) / PUT/PATCH (update) / DELETE (remove)
- Path: `/api/[resource]/[action]` — use plain nouns, no verbs in the path
- Who can call it: any user / logged-in user only / admin only
- What data it receives (request body or URL parameters)
- Data types and validation rules for each field

**Response:**
- Success: what data is returned, what HTTP status code
- Validation errors: which fields failed and why
- Business rule errors: what went wrong (duplicate, not found, unauthorized)
- Server errors: generic error response format

**Format the contract as a table the user can review:**

---
### API: [Plain language name]

**What it does:** [One sentence in plain language]

**Request:**
- Method + Path: `POST /api/customers`
- Requires login: Yes / No
- Data sent:
  | Field | Type | Required | Validation |
  |-------|------|----------|-----------|
  | name | text | Yes | 1–100 characters |
  | phone | text | Yes | Valid phone format |

**Response — success:**
- Status: 201 Created
- Returns: `{ id: 123, name: "...", phone: "..." }`

**Response — errors:**
| Situation | Status | Message shown to user |
|-----------|--------|----------------------|
| Name is empty | 400 | "Please enter a name" |
| Phone format wrong | 400 | "Phone number format is invalid" |
| Phone already exists | 409 | "This phone number is already registered" |
| Server problem | 500 | "Something went wrong, please try again" |

---

### Phase 3 — Review with the user

Present each API contract in plain language. For each one ask:
> "When the user clicks [button], the app will send [data] to the server. The server will [do what] and send back [result]. If [error], the user will see [message]. Does this match what you expect?"

Resolve any "TBD" items before proceeding. Do not implement an API with unresolved questions.

### Phase 4 — Save the API contract

Save to `docs/design/api-contracts.md` (create or append):

```markdown
# API Contracts

## [Feature Name]

### [Endpoint name]

- Method: [METHOD] [/path]
- Auth required: Yes/No
- Behavior contract: docs/contracts/[feature].md

#### Request
[table]

#### Responses
[table]

#### Notes
[Any special behavior, rate limits, external dependencies]
```

### Phase 5 — State next step

> "API contract is saved to `docs/design/api-contracts.md`. Ready to implement? Use the `guided-implementation` skill to build this step by step."
