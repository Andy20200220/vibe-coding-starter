---
name: ui-component-spec
description: 'Use to define a consistent visual language for buttons, forms, colors, typography, and spacing before or during UI implementation. Prevents inconsistent styles across different pages. Keywords: vibe coding, UI, design system, component, button, color, typography, spacing, consistent, style, non-technical.'
argument-hint: 'What to specify, e.g.: define the button styles, establish color palette, create consistent form styles, review the whole app for visual consistency'
user-invocable: true
---

# UI Component Spec Skill

Define or audit the visual rules for common interface elements (buttons, forms, colors, text) so the app looks consistent across all pages.

## When to Use

- Starting a new project and defining the visual style
- Different pages use different button styles, colors, or spacing inconsistently
- The app looks "developer-built" and needs a more coherent visual style
- Before implementing a new screen, to establish what existing components to reuse

## When Not to Use

- A specific layout is broken — use `responsive-layout` skill
- The user wants a full custom design system — that is a larger design project beyond this skill's scope

## Key Concepts (plain language)

- **Design token:** A named value used consistently (e.g., "primary color = #2563EB" used everywhere, not hardcoded separately on each page)
- **Component:** A reusable UI piece (e.g., "primary button" — same style wherever it appears)
- **Visual hierarchy:** Making important things look more important than less important things

## Procedure

### Phase 1 — Understand the context

Ask if not specified:
- Is this a new project (defining styles from scratch) or an existing app (auditing consistency)?
- Does the user have a color preference or existing brand colors?
- What CSS framework is in use? (Tailwind, Bootstrap, plain CSS)
- What is the overall mood? (professional/formal, friendly/casual, minimal/clean)

Read `docs/design/tech-selection.md` for the tech stack.

### Phase 2 — For existing apps: audit inconsistencies

Scan the existing UI files for:
- Multiple different button styles (different colors, sizes, border-radius)
- Inconsistent spacing (some sections use 16px padding, others use 24px or 32px)
- Multiple font sizes without a clear hierarchy
- Colors used ad hoc instead of from a defined palette
- Forms that look different on different pages

List all inconsistencies found.

### Phase 3 — Define the component spec

Establish rules for each component type:

---

**Colors**
- Primary (main action color): `[hex]`
- Primary hover state: `[hex]`
- Danger/destructive: `[hex]` (for delete, error)
- Text (main): `[hex]`
- Text (secondary/muted): `[hex]`
- Background: `[hex]`
- Border: `[hex]`

**Typography**
- Heading 1 (page title): [size], [weight]
- Heading 2 (section title): [size], [weight]
- Body text: [size], [line-height]
- Small/caption: [size]
- Font family: [name] (use system fonts or a single Google Font)

**Spacing scale** (use multiples of 4px)
- XS: 4px | SM: 8px | MD: 16px | LG: 24px | XL: 32px | 2XL: 48px

**Buttons**
- Primary: [background], [text color], [border-radius], [padding], [font-weight]
- Secondary/outline: [border color], [text color], [background]
- Danger: [background], [text color]
- Disabled state: [opacity or color change]
- Minimum size: 44px tall for touchability

**Form inputs**
- Border: [color] [width] [radius]
- Focus state: [border color or outline]
- Error state: [border color], error message placed [below field]
- Placeholder text color: [color]
- Input height: [px]

**Cards/containers**
- Border-radius: [px]
- Shadow: [value or none]
- Padding: [value]

---

Present the spec to the user for approval. Adjust based on feedback.

### Phase 4 — Implement

Apply the spec across the app:
1. Create a central CSS/token file with all defined values (prevents future drift)
2. Replace inconsistent styles with the spec values, one component type at a time
3. Change no more than 3 files per step
4. Verify after each step that the visual change looks correct

### Phase 5 — Document

Save the final spec to `docs/design/ui-component-spec.md` for future reference.

> "Whenever you add a new page or component, refer to `docs/design/ui-component-spec.md` to ensure it matches the established visual style."

### Phase 6 — Save

> "UI component spec applied. Commit: `Apply consistent UI component styles`"
