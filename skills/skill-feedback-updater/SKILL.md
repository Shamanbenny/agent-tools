---
name: skill-feedback-updater
description: Turn approved user feedback about a previously used skill into a concise, reusable improvement.
---

Use this skill when a user gives feedback about how a skill behaved and the feedback might generalize.

## Required Workflow

1. Identify the target skill. If multiple skills are plausible, ask which one.
2. Ask for permission before editing any skill:
   - `Yes (Recommended)` when the feedback is broadly reusable
   - `No`
3. If approved, generalize the feedback into a rule that applies across similar tasks.
4. Update the smallest correct location, preferring `SKILL.md`.
5. Preserve existing behavior and avoid unrelated rewrites.
6. Report which skill changed and what general rule was added.

Do not encode one-off identifiers, temporary repository state, or story-specific assumptions. If feedback is too local to generalize safely, keep it task-local.
