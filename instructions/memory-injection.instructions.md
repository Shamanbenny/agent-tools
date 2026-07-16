# Memory Injection

```text
NOTES_ROOT=~/.agent-notes
STATE_FILE=$NOTES_ROOT/state.json
ACTIVE_MEMORY_FILE=$NOTES_ROOT/SESSION-MEMORY.md
```

Change these values to match the user's environment before using this instruction.

At the start of a session, if `ACTIVE_MEMORY_FILE` from the memory skill configuration exists, read it and use it as additional context for the current task.

If the file does not exist, proceed normally.
