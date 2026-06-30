---
name: accessibility-check
description: 'Use to check and fix accessibility issues: keyboard navigation, screen reader support, color contrast, and ARIA labels. Makes the app usable by people with disabilities and improves overall usability for everyone. Keywords: vibe coding, accessibility, a11y, keyboard, screen reader, contrast, ARIA, usability, non-technical.'
argument-hint: 'What to check, e.g.: the whole app, the login form, the navigation menu, before launch'
user-invocable: true
---

# Accessibility Check Skill

Check the app for accessibility issues and fix them. Makes the app usable with keyboards, screen readers, and for users with visual impairments. Also improves usability for everyone.

## When to Use

- Before deploying or sharing the app publicly
- After building a new feature, as a final quality check
- When a user reports difficulty using the app (keyboard, zoom, contrast)
- When the user specifically wants the app to be accessible

## When Not to Use

- The feature is not built yet — build it first, then check accessibility
- A functional bug is preventing use — fix the bug first

## Key Concepts (plain language)

- **Keyboard navigation:** Can you use the whole app using only the Tab, Enter, and arrow keys? (no mouse)
- **Screen reader:** Software that reads the page aloud for blind users — requires proper labels on all elements
- **Color contrast:** Text must be dark enough against its background to be readable by people with low vision
- **Focus indicator:** The visible outline showing which element is currently selected when using keyboard
- **ARIA label:** A hidden text label on a button or icon that screen readers read aloud

## Procedure

### Phase 1 — Scope the check

Ask if not specified: should I check (a) the whole app, (b) a specific page or feature, or (c) just the most critical paths (login, main action)?

Read the relevant source files and behavior contracts.

### Phase 2 — Run the audit

Check each category:

**A. Keyboard navigation**
- Can every interactive element (buttons, links, inputs, dropdowns) be reached by pressing Tab?
- Can every action be performed without a mouse? (click = Enter, dropdowns = arrow keys)
- Is the Tab order logical? (top to bottom, left to right)
- Can the user get "trapped" in any component (no way to exit with keyboard)?

**B. Focus visibility**
- Is there always a visible outline or highlight showing which element is focused?
- Was the default focus outline removed without a replacement? (CSS `outline: none` without alternative)

**C. Screen reader support**
- Do all images have alt text? (or `alt=""` if decorative)
- Do all icon-only buttons have an accessible label? (e.g., a trash icon button needs `aria-label="Delete"`)
- Do form inputs have associated labels? (`<label for="...">` or `aria-label`)
- Are error messages linked to their input fields? (`aria-describedby`)
- Are page sections marked with landmarks? (`<nav>`, `<main>`, `<header>`, `<footer>`)

**D. Color contrast**
- Does body text meet 4.5:1 contrast ratio against its background?
- Do large headings meet 3:1 contrast ratio?
- Does placeholder text in inputs have sufficient contrast?
- Are links distinguishable by more than just color?

**E. Interactive element size**
- Are all clickable/tappable targets at least 44×44px?

**F. Motion and animation**
- Does any animation play continuously without a way to pause it?
- Does the app respect `prefers-reduced-motion` setting?

### Phase 3 — Report findings

Rate each issue: 🔴 Blocks access / 🟡 Significant barrier / 🟢 Minor improvement

---
## Accessibility Report

### 🔴 Blocks Access
| Issue | Location | How to fix |
|-------|----------|-----------|

### 🟡 Significant Barriers
| Issue | Location | How to fix |
|-------|----------|-----------|

### 🟢 Minor Improvements
| Issue | Location | How to fix |
|-------|----------|-----------|

---

### Phase 4 — Fix blockers first

Address 🔴 blockers one at a time using the standard flow: announce, change (max 3 files), explain, verify, wait for confirmation.

For keyboard navigation verification:
> "Put your mouse aside. Press Tab repeatedly and check that you can reach [element]. Press Enter to activate it. You should [expected result]."

### Phase 5 — Save

> "Accessibility fixes complete. Commit: `Fix accessibility issues in [scope]`"
