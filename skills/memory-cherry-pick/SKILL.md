---
name: memory-cherry-pick
description: Curate session memory by selecting the most relevant entries from an archive and preparing active memory for a fresh agent session.
---

## Configuration

```text
NOTES_ROOT=~/.agent-notes
STATE_FILE=$NOTES_ROOT/state.json
ARCHIVE_FILE=$NOTES_ROOT/SESSION-MEMORY_archive.md
ACTIVE_MEMORY_FILE=$NOTES_ROOT/SESSION-MEMORY.md
```

Change these values to suit the user's environment. Keep them aligned with `personal-doc` and `memory-capture`.

## Instructions

1. Extract the repository, component, task goal, and technical keywords from the current prompt.
2. Read `ARCHIVE_FILE` if it exists. Do not read repositories or begin the actual task.
3. Select 2-5 entries that directly support the task, newest first. If no entries match, leave active memory empty.
4. Clear and rewrite `ACTIVE_MEMORY_FILE` using this structure:

```markdown
# Personal Development Memory — Session Ready

**Cherry-Picked**: [ISO_TIMESTAMP]
**For Task**: [Brief task description]
**Selection Rationale**: [Why these entries were selected]

---

## [TIMESTAMP] - [MODULE/TOPIC]: Entry Title

### Context
[Copied from the archive]

### Key Insight
[Copied from the archive]

### Related Knowledge
[Copied from the archive]
```

Copy selected entry content exactly. Include subsections when present.

5. Update `STATE_FILE` with `lastMemoryCherryPick` and `activeBreadcrumbs`.

This skill only curates memory. It does not perform the underlying task.
