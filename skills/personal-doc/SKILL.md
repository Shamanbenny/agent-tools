---
name: personal-doc
description: Maintain persistent, curated workflow notes in a configurable notes directory for debugging, FAQs, design discussions, and other work where durable context is useful.
---

## Configuration

```text
NOTES_ROOT=~/.agent-notes
STATE_FILE=$NOTES_ROOT/state.json
```

Change these values to suit the user's environment. Use the same values consistently across this skill and the memory skills.

## Primary Goal

Capture the minimum durable context that will help future work:

- current problem framing
- decisions and rationale
- debugging findings or design conclusions
- next useful steps
- reusable project or workflow facts

Do not produce noisy append-only journals. Curate and compress.

## Workflow

### 1. Load state first

Read `STATE_FILE` before choosing a note.

Use it to find:

- the current user or workspace metadata
- the indexed `interesting_files` list and descriptions
- whether a matching file is a maintained note or a legacy reference

If the file is missing, create it using the schema in `references/state-schema.md`.

### 2. Understand the active context

Build the smallest useful context model for the current task:

- current repository or codebase
- current branch when relevant
- active skill or workflow
- whether the task is a debug log, FAQ, design discussion, refinement session, or general working note

Do not fetch external systems unless they materially improve the note.

### 3. Select an existing note before creating a new one

Selection order:

1. Check `interesting_files` descriptions in `STATE_FILE`.
2. Prefer an existing maintained note whose description or topic already fits the current task.
3. Treat legacy files as source material only; do not copy their structure as the standard.
4. Use filename/topic similarity as a secondary signal.
5. Create a new note only when no maintained note is a reasonable fit.

When creating a new note, immediately add it to `STATE_FILE` with a short description, topics, and `format: "standard"`.

### 4. Use the standard note shape

Every maintained note should start with:

1. a metadata table
2. a relevant issues or references table when useful
3. standard section headings chosen from `references/note-template.md`

Use only sections that help the current note. Legacy files do not override this rule.

### 5. Write for reuse, not completeness

- Summarize repeated facts instead of logging every turn.
- Update existing sections instead of stacking duplicate notes.
- Replace stale hypotheses with current understanding.
- Record why a decision was made if that rationale will matter later.
- Keep confidential or transient secrets out of notes.

### 6. Maintain the state catalog

Whenever you create a persistent note or meaningfully repurpose one:

- update `last_updated`
- keep the description current
- add concise topic tags
- mark whether the file is `standard` or `legacy-reference`
- keep `interesting_files` curated

Do not let `STATE_FILE` become a dump.

## Writing Rules

- Prefer concrete facts over narration.
- Prefer stable headings over timestamped bullet spam.
- Prefer one durable note per topic over many tiny fragments.
- Keep notes outside the repository unless the user explicitly wants project documentation.

## Output Expectations

When this skill is active:

- identify whether an existing note should be reused
- create or update the right persistent note under `NOTES_ROOT`
- keep the note tidy and structured
- update `STATE_FILE` if the note inventory changed
