---
name: atomic-usage
description: >
  Explain how to use Atomic Claude, set up a repository for Atomic, install the
  curated Atomic skills, choose an Atomic command, or distinguish Codex support
  from Claude Code integration. Use for Atomic Claude usage and onboarding
  questions, including requests to apply that setup. Does not replace the Atomic
  debugging, TDD, review, or verification skills for actual coding work.
---

# Atomic Claude usage

Give a repo-agnostic answer from this guide first. Do not fetch upstream source
for routine setup or command-selection questions. Check the local installation
only for facts that vary: available skills, CLI version/help, and repo state.
Consult upstream only for a requested update, missing capability, or a version
mismatch that this guide and local help cannot resolve.

For slash-command or binary-subcommand questions, and whenever explaining wiki
setup/refresh, read the bundled [command map](references/commands.md). It covers
the native command catalog, every CLI family in the checked version, the repo
versus realm pipelines, and their side effects. It travels with this skill;
the original agent-tools checkout and upstream website are not required.

This guide describes the curated Codex pack based on upstream commit
`b4ca195cae6022464b0cc5c07273b3fa9602da17`; CLI syntax was checked with Atomic
6.10.0. Treat these as provenance, not an assertion about the user's install.

## Identify the integration

| Layer | What it provides | Required setup |
| --- | --- | --- |
| Curated Codex skills | Debugging, TDD, review, verification, Git text, visual comparisons | Install skill directories; no Atomic binary needed for the core guidance |
| Standalone Atomic CLI | Code graph, queries, repo configuration, local viewer | Install `atomic`; index the target repo for graph queries |
| Native Claude Code bundle | Slash-command workflows, wiki generation, hooks, agents | `atomic claude install`, then use Claude Code |

Having one layer does not imply the others are installed. `$atomic-tdd` is Codex
skill syntax. `/setup-wiki`, `/atomic-plan`, and `/implement` are Claude Code
commands; do not present them as available Codex commands. Agent Markdown files
in the curated pack are role prompts, not registered Codex subagents.

## Usage advice versus execution

For a question, explain the smallest useful setup without installing or editing.
For a request to set it up, inspect and reuse the repo's conventions, then make
the authorized changes. Do not install the Claude bundle or user-wide hooks as
a side effect of setting up Codex skills.

Useful read-only checks when tailoring the answer:

```sh
command -v atomic
atomic --no-update-check --version
atomic --no-update-check where --json
```

If Atomic is absent, still explain or use the standalone skills. If syntax
differs, use `atomic <command> --help`; do not guess flags or run an update.

## Install the curated Codex skills

Use an actual checkout path for `agent_tools_root`; it is the source repo, not
the project receiving the skills. Run this from the target project for a repo
install. Use `destination="$HOME/.agents/skills"` for a user-wide install.

```sh
agent_tools_root=/absolute/path/to/agent-tools
destination="$PWD/.agents/skills"
mkdir -p "$destination"
for source in "$agent_tools_root"/skills/atomic_claude/skills/*; do
  name=$(basename "$source")
  if [ -e "$destination/$name" ] || [ -L "$destination/$name" ]; then
    printf 'Skipped existing skill: %s\n' "$name"
  else
    cp -R "$source" "$destination/$name"
  fi
done
```

Restart or reload the agent to discover the skills. This installs `atomic-usage`
too. To update an existing copy, compare it first and preserve local edits.
Do not tell users to copy the enclosing `atomic_claude` directory as a skill;
its discoverable skills are the individual directories under `skills/`.

The optional role prompts remain in the source pack's `agents/` directory.
Reference their resolved path when useful; do not assume installing skills also
installed the prompts or enabled delegation.

## Prepare any repository

Start with a small root `AGENTS.md` for Codex, reusing one if present. Record:

- The actual stack, repository map, and important domain constraints.
- Real install, development, focused-test, full-test, lint, typecheck, and build
  commands from the repo; omit commands that do not exist.
- When to use Atomic debug/TDD/review/verify and links to existing architecture
  or contribution docs.

Do not invent an npm setup, test baseline, or passing checks. `docs/wiki/`,
`docs/design/`, and `docs/spec/` are optional conventions, not prerequisites for
using the skills. Preserve established equivalents. A Codex-authored architecture
note is useful but is not an Atomic-generated wiki. Keep credentials and private
environment values out of guidance, scans, and generated documentation.

For graph support, install the Atomic CLI only if needed. The upstream installer
is a downloaded shell script: fetch and inspect it before executing it, and use
the user's selected scope. Canonical installer:
`https://raw.githubusercontent.com/damusix/atomic-claude/main/install.sh`.
Then `atomic doctor` checks the installation; it does not prove application tests
pass.

Run graph commands with cwd at the target repo:

| Goal | Command |
| --- | --- |
| Scaffold Atomic repo configuration (optional) | `atomic repo init` |
| Build or rebuild graph | `atomic code index` |
| Check graph availability | `atomic code status` |
| Refresh after code changes | `atomic code sync` |
| Find a behavior | `atomic code explore "how are requests authenticated?"` |
| Find symbol callers | `atomic code callers <symbol>` |
| Check impact | `atomic code impact <symbol> --depth 2` |
| Open local read-only viewer | `atomic serve . --open` |
| Diagnose CLI installation | `atomic doctor` |
| Update CLI (only when requested) | `atomic update` |

The viewer reads available artifacts; it does not create a semantic repo wiki.
Start it only when requested and report the URL actually printed.

## Choose a workflow

| User intent | Codex invocation |
| --- | --- |
| Learn setup or pick a command | `$atomic-usage` |
| Diagnose an unknown failure | `$atomic-debug <symptom>` |
| Implement a feature or known-cause fix | `$atomic-tdd <behavior>` |
| Review a diff | `$atomic-review <scope>` |
| Verify a completion claim | `$atomic-verify <change>` |
| Draft commit or PR text | `$atomic-git-discipline` |
| Compare visual alternatives before editing | `$atomic-visual-options <decision>` |

For a bug: diagnose, write a failing regression test, fix, then verify. For a
feature: establish acceptance criteria, test first, implement, review, verify.
Graph queries support these steps when an index exists; the skills still work
without one. Recommendations do not authorize commits, pushes, or merges.

## Native Claude Code onboarding

If the user wants the full Claude Code integration, install its bundle with
`atomic claude install`. In a Claude Code session at the target repo:

1. `/setup-wiki` audits conventions and scaffolds missing wiki wiring.
2. `/refresh-wiki` builds or refreshes repo knowledge.
3. `/atomic-plan <task>` creates a plan/spec for substantial work.
4. `/implement` or `/subagent-implementation @spec.md` runs the native workflow.

The native repo wiki uses `docs/wiki/` and scoped pointer cards under
`.claude/rules/wiki/`. Existing `CLAUDE.md` guidance must be preserved. In Codex,
link useful generated wiki pages from `AGENTS.md`; Claude rule injection is not
automatically provided by copying the curated skills.

Sources for intentional refreshes: [Atomic Claude](https://github.com/damusix/atomic-claude),
[CLI and workflow docs](https://atomic.alonso.network/). Routine answers should
not require opening these links.
