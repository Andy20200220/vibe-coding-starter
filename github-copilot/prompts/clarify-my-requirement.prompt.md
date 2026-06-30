---
name: "Clarify My Requirement"
description: 'Use when a non-technical user has a new feature idea or product concept that needs to be structured into a clear product definition and behavior contract before any code is written. Keywords: vibe coding, new feature, new product, requirement, idea, behavior contract, PRD, non-technical.'
argument-hint: 'Your idea in plain language, e.g.: I want to build a customer follow-up tracker, add a login feature, build a small tool to manage invoices'
agent: "agent"
---

Help a non-technical user turn a rough idea into a clear product definition and behavior contract.

Requirements:

- Read [workspace instructions](../copilot-instructions-template.md).
- Read the [behavior-contract skill](../skills/behavior-contract/SKILL.md).
- The user does not know code. All communication must use plain, non-technical language.
- Do NOT jump to code, technical design, or implementation. This prompt produces documents only.

## Phase 1 — Understand the idea

Ask the user clarifying questions to understand:
- What problem does this solve? Who uses it?
- Where does it run? (local app, web page, mobile, etc.)
- Single user or multi-user?
- What are ALL the features they can think of? (encourage a complete list, even rough)
- Which 3 features are most important for the first version?

Do not assume answers. Ask explicitly and wait for confirmation.

## Phase 2 — Product definition

Based on confirmed answers, produce a short product definition containing:
- One-sentence product purpose
- Target user
- Platform (where it runs)
- Complete feature list (all features the user mentioned, numbered)
- MVP scope (which 3 features are in the first version, clearly marked)
- Out-of-scope for first version (remaining features, explicitly listed)

Ask the user to confirm or revise this product definition before proceeding.

## Phase 3 — Behavior contract for MVP features

For each MVP feature, produce a behavior contract:
- List every user action (e.g., "user clicks X button")
- For each action, state the expected system response
- For each action, state what happens when something goes wrong (empty input, invalid data, duplicate, etc.)
- State how the user can verify this feature works (specific steps to operate and check)

Use plain language. No technical jargon. No code references.

Ask the user to confirm each behavior contract item by item. Mark any item the user is unsure about as "TBD — needs decision".

## Phase 4 — Save artifacts

- Save the product definition to `docs/contracts/product-definition.md`
- Save each feature's behavior contract to `docs/contracts/<feature-name>.md`
- If a tech stack decision file exists, do not overwrite it. If none exists, note that tech selection is the next step.

Expected output:

- A confirmed product definition document
- One behavior contract per MVP feature, each confirmed by the user
- Clear statement of what the next step is (usually: tech selection → project init → implement first feature)
