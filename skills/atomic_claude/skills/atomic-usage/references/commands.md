# Atomic command map

## Scope and provenance

This is a usage reference, not an implementation of Claude slash commands in
Codex. The agent-tools extraction includes six workflow skills, six role prompts,
and this usage skill. It does not vendor native commands, wiki agents, hooks,
output styles, or the Atomic binary. Install the native bundle to execute its
workflows in Claude Code; do not rename a slash command to `$...` and assume a
Codex skill exists.

Catalog checked against Atomic 6.10.0 (`82246ab`), its
`atomic claude install --dry-run --no-hooks` inventory, local CLI help, and the
upstream command/wiki documentation on 2026-10-23. The older extraction pin
applies to the six adapted workflow skills, not this newer command inventory.
This is a maintained snapshot, not a promise of parity with future releases.

Use this file for routine answers. For a mismatch, read local help first:

```sh
atomic --no-update-check --version
atomic --help
atomic <family> --help
atomic <family> <verb> --help
atomic claude list
```

The last command inspects native installed artifacts. A dry-run installation can
show what the CLI bundles without writing it. Avoid installing a bundle just to
answer a usage question: even `--target` can still use user-wide snapshot state.

## Slash commands: Claude Code workflows

These run in the agent's chat after native bundle installation. They combine
model reasoning, file edits, CLI operations, and sometimes Git/GitHub actions;
there is no general one-to-one executable named after each slash command.

| Command | When to choose it |
| --- | --- |
| `/atomic-help [tour]` | Find the next action or walk through the lifecycle |
| `/setup-wiki` | Audit and bootstrap repo conventions and wiki wiring |
| `/refresh-wiki` | Generate or update repo/realm wiki knowledge |
| `/atomic-plan <task>` | Turn intended work into a plan and checkpoint spec |
| `/gather-evidence <hypothesis>` | Investigate whether evidence supports a premise |
| `/pressure-test` | Challenge a design through dialogue |
| `/challenge-swarm @artifact` | Examine a written proposal through parallel expert perspectives |
| `/implement` | Implement in the main session with checkpoint reviews |
| `/subagent-implementation @spec` | Implement an approved spec through a maker/reviewer loop |
| `/quick-fix <task>` | Apply a known-cause fix without a full spec workflow |
| `/subagent-diagnose` | Investigate and fix a bug or failed CI run |
| `/autopilot <task or issue>` | Orchestrate planning, implementation, review, and shipping |
| `/review-branch` | Review branch changes against the base |
| `/documentation` | Reconcile project documentation with code changes |
| `/commit [push\|pr\|merge\|squash]` | Commit and select the requested shipping extent |
| `/undo-commit` | Soft-undo an eligible local commit |
| `/deslop [path]` | Report maintainability issues in standing code |
| `/deslop apply <selection>` | Fix accepted audit findings |
| `/git-cleanup` | Inspect stale branches/worktrees before authorized removal |
| `/watch-ci` | Monitor the current branch's CI |
| `/remind-me <when> <task>` | Record/schedule a reminder with the available runtime |
| `/follow-up [review]` | Inspect reminders or triage project follow-ups |
| `/session-report` | Record session changes and reasons for later shipping context |
| `/retrospective-learning` | Propose workflow improvements from session feedback |
| `/report-issue` | Prepare/open an issue in the current repository |
| `/report-issue-with-atomic` | Prepare/open an issue about Atomic itself |

This catalog helps choose a workflow; the installed command owns its execution
contract. Native implement/ship/refresh workflows can stage or commit artifacts,
and shipping/issue commands may affect GitHub. Explain relevant effects before
execution and respect the user's requested scope. Explaining a command does not
authorize running it.

## How wiki slash commands use the binary

`/setup-wiki` is an agent-led conventions audit: inspect existing instructions,
ignore rules, docs layout, and wiki steering before scaffolding missing pieces.
`atomic repo init` and `atomic wiki init --scope repo` provide deterministic
scaffolding, but neither performs that audit or writes inferred architecture.

Repo refresh has three distinct parts:

```text
atomic signals scan
  -> docs/wiki/scan.md (facts)
  -> wiki agents infer index/domain pages (meaning)
  -> atomic signals linkify (resolving source citations)
```

The optional code graph supports inference but is a separate artifact. Running
`atomic code index` does not generate wiki prose. Running `atomic signals scan`
alone does not replace `/refresh-wiki` either.

| Repo artifact | Role |
| --- | --- |
| `docs/wiki/scan.md` | Deterministic scan output |
| `docs/wiki/index.md` and domain pages | Agent-authored architecture and domain map |
| `docs/wiki/CLAUDE.md` | Steering for inference; not scan exclusions |
| `.claude/rules/wiki/<domain>.md` | Generated path-scoped pointers loaded by Claude Code |
| `.claude/atomic.toml` | Atomic configuration, including scan exclusions/generated-path rules |

Realm scope maps multiple repos and capture buckets from a parent directory:

```text
atomic wiki scan
  -> member inventory and wiki/ scaffold
  -> wiki agents summarize members and shared concerns
  -> capture-bucket differences inform knowledge pages
  -> atomic wiki stamp / linkify maintain metadata and links
```

`atomic wiki init --scope realm` writes a scope marker/scaffold; it is not the
inference pass. `atomic wiki scan` is the realm scanner and can also register
the realm in user-level Claude guidance. Do not run it at a single repo root
merely because its name sounds like a repo wiki scan. `/refresh-wiki` chooses
scope using the wiki marker; inspect `atomic where --json` when uncertain.

Repo and realm knowledge can coexist: a realm may link a member's repo wiki.
In Codex, CLI scanning/querying still works, and agents can read or author
Markdown, but automatic Claude pointer injection and native wiki orchestration
are not supplied by this curated pack.

## Binary families: terminal tools

These belong to the installed executable, so they do not appear in the slash
menu. Alternatives separated by `/` below are subcommand names, not literal
shell syntax. Use the installed leaf command's help for arguments and flags.

| Family | Subcommands or operation | Purpose |
| --- | --- | --- |
| `atomic code` | `index`, `sync`, `status`, `search`, `callers`, `callees`, `impact`, `node`, `files`, `affected`, `explore`, `comments`, `mcp` | Symbol graph, navigation, changed-code analysis, and MCP serving |
| `atomic wiki` | `init`, `scan`, `stale`, `linkify`, `stamp`, `bucket` | Scope scaffolds, realm scanning, wiki metadata, capture buckets |
| `atomic signals` | `scan`, `show`, `stale`, `diff`, `linkify` | Deterministic repo wiki substrate and freshness |
| `atomic docs` | `scan`, `stale`, `index` | Track documentation surfaces and refresh indexes |
| `atomic repo` | `init` | Repo-local Atomic configuration scaffold |
| `atomic where` | `--json` | Resolve current repo/realm/index and state paths |
| `atomic serve` | viewer; `--open`, `--port`, `--host` | Browse existing wiki/graph locally; read-only, localhost by default |
| `atomic claude` | `install`, `update`, `list`, `diff`, `uninstall` | Manage native Claude artifacts |
| `atomic hooks` | `session-start`, `install`, `uninstall` | Native session-start integration |
| `atomic config` | `get`, `set`, `unset`, `list`, `path`, `agents`, `resolve` | Read/update settings and agent routing |
| `atomic scratchpad` | `new`, `path`, `list`, `archive` | Task working bundles across workflow phases |
| `atomic template` | `design-doc`, `spec`, `brief`, `state`, `followups`, `diagnose-context`, `implementation-log`, `session-report` | Emit workflow document skeletons |
| `atomic prompt` | `git-cleanup`, `claude-merge`, `implementer`, `reviewer` | Emit role/task briefs |
| `atomic validate` | `spec`, `config`, `bundle`, `artifacts` | Validate workflow artifacts and references |
| `atomic doctor` | installation checks | Diagnose Atomic integrity; separate from app validation |
| `atomic update` | self-update and artifact refresh | Update the CLI/native integration intentionally |
| `atomic migrate` | versioned state migration | Migrate Atomic state/layout |
| `atomic bus` | room/session messaging and gateway operations | Coordinate concurrent sessions; not required for single-agent work |
| `atomic repl` | `start`, `eval`, `list`, `status`, `reset`, `stop` | Persistent named Python/Node interpreters |
| `atomic retro` | `extract` | Extract session history for retrospective work |
| `atomic reminder` | `add`, `list`, `show`, `rm` | Maintain reminder records; scheduling depends on runtime |
| `atomic followups` | `list`, `add`, `close`, `render`, `path` | Maintain typed project follow-ups |
| `atomic profile` | `refresh` | Refresh recorded user environment |
| `atomic docker` | `init` | Scaffold an evaluation environment |

`atomic wiki bucket` includes `add`, `list`, `diff`, `promote`, `doc`, `skill`,
and `index`: register sources, examine changes, scaffold captures, and advance
their baselines. `atomic wiki stamp` records source fingerprints; it does not
verify that generated prose is semantically accurate.

Freshness commands commonly return 0 for fresh, 1 for stale, and 2 for errors.
Treat a stale result as a maintenance signal, not automatically an execution
failure. Check leaf help because status contracts differ.

## Practical combinations

| Need | Workflow and tools |
| --- | --- |
| First native repo onboarding | `/setup-wiki`, then `/refresh-wiki`; optionally index code |
| Refresh repo facts only | `atomic signals scan`; no inference claimed |
| Rebuild a repo's knowledge pages | `/refresh-wiki`; scan + wiki agents + linkification |
| Map a folder of related repos | Realm setup/refresh using `atomic wiki` plus wiki agents |
| Investigate callers and blast radius | `atomic code index` once, `sync` after edits, then queries |
| Browse knowledge | `atomic serve . --open` after artifacts exist |
| Track durable architecture decisions | Existing project docs, optionally `docs/design/` and `docs/spec/` |
| Locate transient reports and archives | `atomic where --json`; do not construct home-state paths |

Do not run hooks, a bus, a server, migrations, updates, or Git operations as a
side effect of a usage answer. Keep advice proportional to the requested task.

For deliberate refreshes of this reference:
[command catalog](https://github.com/damusix/atomic-claude/blob/main/docs/reference/commands.md),
[repo wiki](https://github.com/damusix/atomic-claude/blob/main/docs/reference/repo-wiki.md),
[realm wiki](https://github.com/damusix/atomic-claude/blob/main/docs/reference/realm-wiki.md).
