# Atomic Claude skill and agent pack

This directory contains a Codex-friendly, curated extraction from [damusix/atomic-claude](https://github.com/damusix/atomic-claude), pinned to commit `b4ca195cae6022464b0cc5c07273b3fa9602da17`.

## Contents

### Skills

- `atomic-debug` - diagnose unknown failures through testable hypotheses.
- `atomic-git-discipline` - write concise commit and pull request text.
- `atomic-review` - report focused, actionable review findings.
- `atomic-tdd` - implement behavior test first.
- `atomic-verify` - verify claims with fresh evidence.
- `atomic-visual-options` - compare visual directions before implementation.

The upstream `atomic-bus` skill assumes Claude Code session hooks, and the wiki, documentation, and writing skills are outside this curated pack.

### Agent role prompts

The `agents/` directory contains six portable role prompts: auditor, deslopper, implementer, investigator, reviewer, and strategist. They are Markdown instructions, not Codex agent configuration files; give the relevant file to a delegated agent as its task instructions. The upstream wiki-only agents are not included.

## Use with Codex CLI

Install the skill directories into a Codex-discovered skill directory, then restart Codex:

```sh
mkdir -p .agents/skills
cp -R skills/atomic_claude/skills/* .agents/skills/
```

Or make them available to all projects:

```sh
mkdir -p ~/.agents/skills
cp -R skills/atomic_claude/skills/* ~/.agents/skills/
```

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

These files are adapted from the upstream Atomic Claude skills and agents. See [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) for the MIT notice and source paths. The project's native CLI documentation remains at [atomic.alonso.network](https://atomic.alonso.network/).
