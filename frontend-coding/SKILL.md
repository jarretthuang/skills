---
name: frontend-coding
description: Implement and refine frontend/UI code for React, Next.js, component libraries, styling systems, and interaction polish. Use when building or iterating on UI components, layouts, styling, accessibility-facing interaction behavior, visual polish, responsive behavior, or design-system alignment. Especially use when touching CSS, Tailwind, component props/classes, icons, spacing, typography, or other presentational code where matching existing patterns matters.
---

# Frontend Coding

Implement frontend changes in a way that fits the product's existing visual and interaction system.

## Core rule

Before changing UI/CSS, inspect the repo's existing component and style patterns first.

Match the established visual system before introducing new styling, spacing, icon treatments, interaction states, or one-off abstractions.

## Workflow

1. Define the user-visible problem in one sentence.
2. Inspect nearby components, shared primitives, and existing patterns before writing code.
3. Reuse the closest existing pattern unless there is a clear reason not to.
4. Keep the diff minimal and local when the task is visual polish or interaction cleanup.
5. Preserve accessibility and keyboard behavior while adjusting UI.
6. Add lean tests when behavior changed; use the frontend-testing skill for test scope.
7. Run the narrowest useful checks first, then broader checks as needed.
8. Summarize what pattern you matched, what changed, and any remaining visual risk.

## Pattern check

When touching frontend presentation, look for the repo's existing choices around:

- spacing and sizing
- typography and text hierarchy
- icon size, weight, and button treatments
- hover, focus, active, disabled, and loading states
- border radius, shadows, outlines, and color usage
- responsive layout behavior
- composition through shared primitives or wrappers

Prefer extending those patterns over inventing new local ones.

## Good defaults

- Prefer existing shared components over ad-hoc markup.
- Prefer editing the smallest surface that restores consistency.
- Prefer explicit, readable code over clever styling indirection.
- Prefer design-system consistency over isolated local optimization.

## Avoid

- One-off spacing/icon tweaks that do not match surrounding controls
- New abstractions for tiny presentational fixes
- Restyling a component without checking sibling patterns
- Large UI rewrites when a small consistency fix solves the problem
- Breaking keyboard/focus behavior while polishing visuals

## Output contract

Return concise bullets:
- Problem
- Existing pattern matched
- What changed
- Checks run
- Remaining risk
