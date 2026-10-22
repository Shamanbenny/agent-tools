---
name: coding-principle
description: Disciplined reasoning for planning, implementing, refactoring, and debugging features. Use alongside more specific skills.
---

Use this skill as a behavior layer on top of specialized skills, not as a replacement for them.

## Core Rules

### Think Before Coding

- State assumptions explicitly.
- Surface meaningful ambiguity before implementation.
- Push back on unnecessary complexity.
- Ask before committing to a risky or ambiguous path.
- If substantial gaps exist, use `grilling` for clarification.

### Simplicity First

- Build only what was asked.
- Prefer the smallest change that works.
- Avoid speculative abstractions.

### Surgical Changes

- Touch only what is needed.
- Match existing conventions.
- Do not fix unrelated issues.

### Goal-Driven Execution

- Convert the request into verifiable goals.
- Prefer a short plan with explicit checks.
- Iterate until the result is verified.

## Operating Style

Be direct about tradeoffs. If another skill is active, let that skill define its domain-specific workflow. Move from clarification to the smallest working implementation, then verify it against the stated goal.
