---
name: coding-principles
description: Apply first-principles engineering to coding tasks. Use when implementing features, fixing bugs, evaluating architecture, or responding to code review so solutions are derived from core problem constraints and invariants (not cargo-cult patterns).
---

# Coding Principles

Use this skill as the shared engineering operating system before writing code.

## Core Method

1. Define the problem in one sentence.
2. Identify non-negotiable constraints (correctness, performance, compatibility, UX, delivery).
3. State invariants that must remain true after the change.
4. Name likely failure modes and how to prevent them.
5. Choose the smallest approach that satisfies constraints and invariants.
6. Add lean tests that prove changed behavior and guard key regressions.
7. Validate with targeted checks first, then broader checks as needed.
8. Summarize residual risk and explicit follow-ups.

## Frontend specialization

For frontend/UI work, also read `references/frontend.md` before implementing so changes match the repo's established visual and interaction patterns.

## Decision Gate: Fast Path vs Design Path

Use fast path for small, localized bugs or low-risk improvements.

Switch to design path when scope is broad, requirements are unclear, or trade-offs are material.

For design path, produce a short plan first:
- problem
- constraints/invariants
- options with trade-offs
- recommended approach
- test strategy

## PR Hygiene (when applicable)

- Keep one task per branch.
- Iterate in the same PR while the task is active.
- If the PR is merged, start a new branch and new PR for further work.
- Keep PR description synchronized with latest scope, behavior, tests, and risks.
- Keep review replies minimal and factual.

## Output Contract for Each Task

Return concise bullets:
- Problem
- Constraints & invariants
- Chosen approach
- What changed
- Tests run
- Remaining risk
