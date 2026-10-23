# Atomic Claude skill and agent pack

This directory contains a Codex-friendly, curated extraction from [damusix/atomic-claude](https://github.com/damusix/atomic-claude), pinned to commit `b4ca195cae6022464b0cc5c07273b3fa9602da17`.

## Contents

### Skills

- `atomic-usage` - repo-agnostic setup, command selection, and Codex versus Claude Code guidance (authored here).
- `atomic-debug` - diagnose unknown failures through testable hypotheses.
- `atomic-git-discipline` - write concise commit and pull request text.
- `atomic-review` - report focused, actionable review findings.
- `atomic-tdd` - implement behavior test first.
- `atomic-verify` - verify claims with fresh evidence.
- `atomic-visual-options` - compare visual directions before implementation.

### Native commands and binary subcommands

This is a curated extraction, not the full Atomic Claude distribution. Native slash-command implementations (including `/setup-wiki` and `/refresh-wiki`), wiki agents, hooks, output styles, and the binary are not vendored here. Earlier versions documented only a few of these in this README; that left a usage gap.

The bundled [`atomic-usage` command map](skills/atomic-usage/references/commands.md) now accounts for the native slash-command catalog and all 24 CLI families in the checked version, including `atomic code`, `atomic wiki`, and `atomic signals`. It explains how repo and realm wiki workflows combine deterministic tools with agent inference, and which commands can write files, install integration, or commit work. Copying the usage skill carries this reference with it; it does not port native execution to Codex.

The upstream `atomic-bus`, `atomic-wiki`, `atomic-documentation`, and `atomic-writing` skills remain outside the extracted executable guidance. Install the native bundle for their Claude Code behavior.

### Agent role prompts

The `agents/` directory contains six portable role prompts: auditor, deslopper, implementer, investigator, reviewer, and strategist. They are Markdown instructions, not Codex agent configuration files; give the relevant file to a delegated agent as its task instructions. The upstream wiki-only agents are not included.

## Use with Codex CLI

Install the individual skill directories into a Codex-discovered directory. Set the source checkout path explicitly so this works from any target repository. Existing copies are preserved:

```sh
agent_tools_root=/absolute/path/to/agent-tools
destination="$PWD/.agents/skills"
# Or: destination="$HOME/.agents/skills" for all projects.
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

Restart or reload Codex. For setup questions, invoke `$atomic-usage` or ask naturally how to use Atomic Claude. Its [self-contained guide](skills/atomic-usage/SKILL.md) covers installation scope, repository instructions, graph queries, and native Claude Code onboarding without an upstream lookup. Compare existing copies before intentionally updating them.

Invoke a skill with Codex syntax such as `$atomic-debug` or `$atomic-tdd`.

For an agent role, ask Codex to use a prompt file, for example: "Use `skills/atomic_claude/agents/atomic-reviewer.md` as the reviewer instructions for this diff."

## Atomic CLI notes

The extracted skills can use Atomic's standalone CLI when installed. They do not require the Claude Code bundle for code indexing or serving:

```sh
atomic code index
atomic code sync
atomic code explore "where is request authentication handled?"
atomic serve . --open
```

The native Atomic Claude commands such as `/setup-wiki` and `/refresh-wiki`, session hooks, and `atomic claude install` install Claude Code integration. They are not Codex CLI setup commands.

## Provenance

The six workflow skills and six agent prompts are adapted from upstream Atomic Claude. `atomic-usage` is an original companion guide authored here; its CLI/version basis is recorded in the skill. See [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) for the MIT notice and source paths. The project's native CLI documentation remains at [atomic.alonso.network](https://atomic.alonso.network/).
