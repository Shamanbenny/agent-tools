# Note Injection

```text
NOTES_ROOT=~/.agent-notes
STATE_FILE=$NOTES_ROOT/state.json
```

Change these values to match the user's environment before using this instruction.

When the user starts a task or clearly changes tasks, read the configured notes `STATE_FILE` and inspect its `interesting_files` list.

Load only notes that are genuinely relevant to the current task. If a relevant note is loaded, briefly tell the user that useful context was injected before proceeding. If nothing is relevant, continue without comment.
