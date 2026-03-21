---
name: frontend-testing
description: Write and refine frontend tests for React/Next.js/UI code. Use when adding or updating tests for components, hooks, utilities, rendering behavior, or interaction behavior, especially when deciding test scope, keeping tests lean, or choosing between unit and DOM coverage.
---

# Frontend testing

Write the smallest test that protects the intended behavior.

## Principles

- Keep tests lean and focused.
- Prefer behavior over implementation details.
- Test user-visible contracts, not internal structure.
- If a feature is intentionally approximate, test the chosen simplification.
- Prefer product-relevant coverage over speculative edge cases.

## Coverage order

1. Pure helpers and parsing/formatting logic
2. Hook logic and state transitions
3. User-visible component behavior
4. Broader DOM/integration flows only when necessary

Prefer extracting small testable logic over forcing heavy UI harnesses.

## Good tests

- Small and direct
- Tied to a real behavior change
- Focused on one contract
- Stable across harmless refactors

Good targets:
- labels/text
- state derivation
- branching behavior
- event outcomes
- accessibility-relevant behavior

## Avoid

- Large snapshots
- Private implementation detail tests
- Low-value edge-case suites
- Heavy browser-style setups for tiny utility changes

## Decision check

Before writing a test, ask:

1. What is the contract?
2. What is the smallest test that proves it?
3. Does this need DOM rendering?
4. Will this still make sense after a refactor?
5. Does this edge case actually matter to users?

## Heuristic features

For approximate features like read-time, truncation, ranking, or fuzzy formatting:

- Prefer a simple stable rule
- Test that rule directly
- Do not overfit to theoretical edge cases unless they matter to users

## Workflow

1. Identify the behavior or invariant
2. Choose the lightest useful test level
3. Add one or two focused tests
4. Run the narrowest useful check first
5. Expand only if there is a real uncovered risk

## Bias

For small frontend utility changes, prefer a tiny direct test runnable with Node if possible.

For presentational or interaction-heavy behavior, use the repo’s existing frontend test stack.
