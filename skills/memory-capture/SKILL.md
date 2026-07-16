---
name: memory-capture
description: Capture session insights into an append-only archive file with minimal token usage. Use at the end of a session to preserve useful technical, architectural, or process learnings.
---

## Configuration

```text
NOTES_ROOT=~/.agent-notes
STATE_FILE=$NOTES_ROOT/state.json
ARCHIVE_FILE=$NOTES_ROOT/SESSION-MEMORY_archive.md
```

Change these values to suit the user's environment. Keep them aligned with `personal-doc` and `memory-cherry-pick`.

## What to Capture

Focus on useful future context:

- module or component context
- technical learnings and gotchas
- architecture and data-flow insights
- effective development, testing, or debugging workflows

## Archive File Format

The archive is append-only:

```markdown
# Personal Development Memory Archive

**Last Updated**: YYYY-MM-DD HH:MM
**Total Entries**: N

---

## [TIMESTAMP] - [MODULE/TOPIC]: Brief Title

### Context
What task were you working on?

### Key Insight
The actual learning or discovery.

### Related Knowledge
- Links to existing notes or related topics
```

## Implementation Instructions

1. Read `STATE_FILE` and `ARCHIVE_FILE` immediately, if they exist. Use only those files for this initial step.
2. Analyze the task context already provided; do not reread the conversation from disk.
3. Identify novel, reusable insights not already in the archive.
4. Append structured entries in one write, update the archive timestamp and count.
5. Update `STATE_FILE` atomically with `lastMemoryCapture` and `archiveEntryCount`.

Keep each entry self-contained and concise. Never modify existing archive entries.

## Archive vs Active Memory

- `SESSION-MEMORY_archive.md` is the complete append-only history.
- `SESSION-MEMORY.md` is a curated subset prepared by `memory-cherry-pick`.
