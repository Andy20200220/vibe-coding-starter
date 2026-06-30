---
name: tech-selection
description: 'Use when a non-technical user has a confirmed product definition and needs to choose a technology stack before any code is written. Asks about constraints, recommends a concrete stack in plain language, explains tradeoffs, and saves the decision for future reference. Keywords: vibe coding, tech selection, technology, framework, stack, platform, non-technical, recommend, plain language.'
argument-hint: 'Product definition or idea to select tech for, e.g.: my customer follow-up tracker, the product defined in docs/contracts/product-definition.md'
user-invocable: true
---

# Tech Selection Skill

Help a non-technical user choose the right technology stack for their project by asking about constraints, recommending a concrete option in plain language, and saving the decision.

## When to Use

- A product definition is confirmed (in `docs/contracts/product-definition.md` or discussed in chat)
- No technology has been chosen yet
- The user does not know what framework, language, or platform to use

## When Not to Use

- The product idea is still vague — use `Clarify My Requirement` prompt first
- Tech has already been selected — check `docs/design/tech-selection.md`
- The user is asking about a specific technical concept, not selecting a stack

## Procedure

### Phase 1 — Understand constraints

Ask the user the following questions. Do not assume answers. Wait for replies before making recommendations.

1. **Platform:** Where should this run?
   - Local app on my computer
   - Web page (open in browser, anyone can access)
   - Mobile app (iPhone / Android)
   - Not sure

2. **Computer:** What computer do you use?
   - Windows
   - Mac
   - Both

3. **Users:** Who will use this?
   - Just me
   - A small team (2–10 people)
   - Anyone on the internet

4. **Data storage:** Where should data live?
   - On my computer only (local file)
   - In the cloud (accessible from anywhere)
   - Not sure

5. **Server:** Do you have a server or hosting account?
   - No, I want everything to run on my computer
   - Yes, I have (or can get) a hosting account
   - Not sure

6. **Future scale:** Optional but helpful — do you expect this to grow significantly or stay small?

### Phase 2 — Read the product definition

Read `docs/contracts/product-definition.md` if it exists. Note:
- Total feature count (full product, not just MVP)
- Any features that have specific technical implications (e.g., real-time updates, file uploads, email notifications, payments)
- MVP scope

Make recommendations based on the **full product feature list**, not just the MVP. Tech selected for 3 features may be wrong for 15.

### Phase 3 — Recommend a stack

Based on constraints and the product definition, recommend **one concrete stack**. Do not present a menu of options without a clear recommendation — the user cannot evaluate technical tradeoffs.

Format the recommendation like this:

---
**My recommendation: [Stack Name]**

What it is (in plain language):
- [Component 1]: [what it does in plain language, no jargon]
- [Component 2]: [what it does in plain language]
- [Component 3]: [what it does in plain language]

Why this fits your project:
- [Reason 1 tied to a specific user constraint or product requirement]
- [Reason 2]
- [Reason 3]

What it cannot do well (honest tradeoffs):
- [Limitation 1]
- [Limitation 2]

What you'll need to install:
- [Tool 1] — [one sentence on what it is]
- [Tool 2] — [one sentence on what it is]

---

Ask the user: "Does this make sense? Do you want to go ahead with this, or is there a constraint I missed?"

If the user raises a constraint that changes the recommendation, revise and recommend again.

### Phase 4 — Save the decision

Once the user approves, save the decision to `docs/design/tech-selection.md`:

```markdown
# Tech Selection

Date: YYYY-MM-DD
Status: Confirmed

## Stack

- [Component]: [name and version if known]
- [Component]: [name and version if known]

## User Constraints

- Platform: [answer]
- Computer: [answer]
- Users: [answer]
- Data storage: [answer]
- Server: [answer]

## Why This Stack

[Summary of reasoning in plain language]

## Known Limitations

[Honest list of what this stack does not do well]

## Installation Required

- [Tool]: [how to install — one line]
- [Tool]: [how to install — one line]
```

### Phase 5 — State next step

After saving:
> "Tech selection is saved to `docs/design/tech-selection.md`. The next step is to initialize the project — I'll create the folder structure and make sure it runs. Ready to start?"
