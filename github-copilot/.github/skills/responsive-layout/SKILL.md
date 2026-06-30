---
name: responsive-layout
description: 'Use when a page looks wrong on mobile, tablet, or different screen sizes. Fixes layout so it adapts to any screen without breaking the desktop version. Keywords: vibe coding, responsive, mobile, tablet, layout, screen size, adapt, breakpoint, non-technical.'
argument-hint: 'What looks wrong, e.g.: the customer list is cut off on mobile, buttons overlap on small screens, the sidebar disappears on tablet'
user-invocable: true
---

# Responsive Layout Skill

Fix pages so they display correctly on all screen sizes — mobile, tablet, and desktop — without breaking existing layouts.

## When to Use

- A page looks wrong on a phone or tablet
- Buttons, text, or images are cut off or overlapping on small screens
- A new feature needs to work on both desktop and mobile from the start
- The user wants to check if the app is mobile-friendly before sharing it

## When Not to Use

- The issue is a broken feature, not a layout problem — use `Diagnose and Fix` prompt
- The user wants to change the visual design style — use `ui-component-spec` skill

## Key Concepts (plain language)

- **Breakpoint:** A screen width where the layout switches (e.g., below 768px = mobile layout)
- **Responsive:** The layout adjusts automatically based on screen size
- **Overflow:** Content is wider than the screen and gets cut off or causes scrolling

## Procedure

### Phase 1 — Identify the problem

Ask the user:
- Which page or feature looks wrong?
- Which device/screen size is the problem? (phone, tablet, small laptop)
- What exactly looks wrong? (cut off, overlapping, too small to tap, broken columns)

If possible, ask the user to describe what they see vs. what they expect to see.

Read the relevant source files for the affected page.  
Check what CSS framework is in use (Tailwind, Bootstrap, plain CSS, etc.) from `docs/design/tech-selection.md`.

### Phase 2 — Audit the layout

Check the following on the affected page:

**A. Overflow issues**
- Are there fixed widths that exceed mobile screen width? (e.g., `width: 800px` on a container)
- Are there elements that do not wrap or shrink on small screens?
- Is horizontal scrolling appearing when it should not?

**B. Touch targets**
- Are buttons and links at least 44x44px on mobile? (too small = hard to tap)
- Are interactive elements too close together?

**C. Text readability**
- Is font size too small on mobile? (minimum 16px for body text)
- Is line length too long on wide screens? (ideal: 60–80 characters per line)

**D. Images and media**
- Do images scale down on small screens or overflow?
- Are there fixed-size images that should be fluid?

**E. Navigation**
- Does the navigation still work on small screens?
- Is there a hamburger menu or equivalent for mobile?

**F. Column layouts**
- Do multi-column layouts stack to single column on mobile?
- Are table columns readable on small screens, or do they need to restructure?

### Phase 3 — Fix step by step

For each issue found:
1. Announce the change in plain language: "I'm going to make the customer list stack into a single column on mobile instead of two side-by-side columns"
2. Change no more than 3 files per step
3. Explain what changed: "On screens smaller than a phone width, the two columns will now stack on top of each other"
4. Provide verification: "To check: open the page, then resize your browser window to be very narrow (or open on your phone). The columns should now stack vertically"
5. Wait for confirmation

### Phase 4 — Cross-size verification

After all fixes, check three sizes:
- **Mobile:** ~375px wide (iPhone size)
- **Tablet:** ~768px wide
- **Desktop:** ~1280px wide

> "Please check these three sizes and confirm everything looks correct at each one."

### Phase 5 — Save

> "Responsive layout fixes complete. Commit: `Fix responsive layout for [page/feature] on mobile and tablet`"
