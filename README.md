# agent-tools

Reusable agent skills and instructions for personal developer workflows.

## Included

- `skills/grilling` and `skills/grill-with-docs` for design clarification
- `skills/personal-doc` for curated persistent notes
- `skills/memory-capture` and `skills/memory-cherry-pick` for session memory
- `skills/coding-principle` for implementation discipline, inspired by [Andrej Karpathy's skills repository](https://github.com/multica-ai/andrej-karpathy-skills)
- `skills/skill-feedback-updater` for approved, general skill improvements
- `instructions/` for generic note and memory injection

## Configuration

The note and memory skills default to:

```text
NOTES_ROOT=~/.agent-notes
```

Each relevant `SKILL.md` lists its configuration near the top. Change the path there to use another notes directory, and keep the values consistent across the skills.

The port is intentionally tool- and organization-agnostic.
