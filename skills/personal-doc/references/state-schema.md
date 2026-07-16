# State File Schema

Keep the state file small and curated.

## Required Shape

```json
{
  "version": 1,
  "notes_root": "~/.agent-notes",
  "filename_pattern": "YYYYMMDD-HHmm-Short Title.md",
  "filename_pattern_note": "Use a filesystem-safe timestamp format.",
  "interesting_files": [
    {
      "path": "~/.agent-notes/example.md",
      "description": "What this file is for",
      "format": "standard",
      "topics": ["example"],
      "last_updated": "2026-01-01"
    }
  ]
}
```

## Rules

- `interesting_files` is the primary note-selection index.
- Each entry must declare `format` as either `standard` or `legacy-reference`.
- New maintained notes must be `standard`.
- Legacy files may be indexed for discovery, but must not define the template for future notes.
- Keep descriptions short and functional.
- Topic tags should be broad enough to match future work.
- Update `last_updated` when the note meaningfully changes.
- Do not track throwaway scratch files here.
