# agent-tools

Reusable agent skills and companion workflows for personal developer work.

## Included skills

- `skills/grilling` for design clarification
- [`skills/concept-explainer`](https://x.com/karpathy/status/2105819303471976479) for clear, layered explanations that apply ASD-STE100 principles for roughly 80% of the prose
- `skills/coding-principle` for implementation discipline, inspired by [Andrej Karpathy's skills repository](https://github.com/multica-ai/andrej-karpathy-skills)
- `skills/skill-feedback-updater` for approved, general skill improvements
- `skills/atomic_claude` for curated Atomic workflows and `atomic-usage` onboarding
- `skills/impeccable-usage` for repo-agnostic Impeccable setup and command guidance

The coding-principle skill is tool-agnostic. Grilling and skill-feedback-updater use the CLI Agent's **Ask user** tool.

## Companion tools

Setup and usage guidance lives in discoverable skills, so agents can answer ordinary onboarding questions without fetching the original source each time:

| Tool | Usage skill | Covers |
| --- | --- | --- |
| [Atomic Claude](https://atomic.alonso.network/) | [`atomic-usage`](skills/atomic_claude/skills/atomic-usage/SKILL.md) | Skill installation, native slash commands, all CLI families, wiki pipelines, and the Codex/Claude Code boundary |
| [Impeccable](https://impeccable.style/) | [`impeccable-usage`](skills/impeccable-usage/SKILL.md) | Installation scope, PRODUCT.md versus DESIGN.md, command selection, hooks, and maintenance |

The guides are repo-agnostic. Their descriptions enable automatic selection for questions such as “How do I set up this repo for Atomic Claude?” or “How should I use Impeccable here?” Explicit invocations are `$atomic-usage` and `$impeccable-usage`. Automatic selection depends on the agent; it is not a shell hook. The Impeccable companion does not bundle the upstream design skill or launcher.

### Install the usage skills for Codex

Set the source checkout path below. Run from the target repository for a project install, or select the user-wide destination for all projects. Existing copies are skipped so local customizations survive.

```sh
agent_tools_root=/absolute/path/to/agent-tools
destination="$PWD/.agents/skills"
# Or: destination="$HOME/.agents/skills" for all projects.
mkdir -p "$destination"
for source in "$agent_tools_root/skills/atomic_claude/skills/atomic-usage" "$agent_tools_root/skills/impeccable-usage"; do
  name=$(basename "$source")
  if [ -e "$destination/$name" ] || [ -L "$destination/$name" ]; then
    printf 'Skipped existing skill: %s\n' "$name"
  else
    cp -R "$source" "$destination/$name"
  fi
done
```

Reload the agent after installation. Compare existing copies before intentionally updating them. To install the full curated Atomic pack, use the [pack instructions](skills/atomic_claude/README.md); they include `atomic-usage`.

### Example prompts

- `$atomic-usage Explain the smallest useful setup for this repo. Distinguish the skills from optional CLI and Claude Code features.`
- `$atomic-usage Set up repo guidance and the code graph using this project's existing conventions.`
- `$impeccable-usage How should I onboard this existing app without changing its visual identity?`
- `$impeccable-usage Explain init, document, critique, audit, and polish, and recommend where to start.`

Usage questions get advice; setup or design changes run only when requested. Each guide records its version basis and uses installed documentation or local help for drift. Upstream lookup is a fallback for requested updates or unresolved gaps.
