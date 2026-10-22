# agent-tools

Reusable agent skills and companion workflows for personal developer work.

## Included skills

- `skills/grilling` for design clarification
- [`skills/concept-explainer`](https://x.com/karpathy/status/2105819303471976479) for clear, layered explanations that apply ASD-STE100 principles for roughly 80% of the prose
- `skills/coding-principle` for implementation discipline, inspired by [Andrej Karpathy's skills repository](https://github.com/multica-ai/andrej-karpathy-skills)
- `skills/skill-feedback-updater` for approved, general skill improvements
- `skills/atomic_claude` for a curated, Codex-friendly extraction of Atomic Claude skills and agent role prompts

The coding-principle skill is tool-agnostic. Grilling and skill-feedback-updater use the CLI Agent's **Ask user** tool.

## Companion tools

The instructions below target Codex CLI. Each tool has its own setup steps and prompt examples so another coding agent can be documented alongside it later.

### Impeccable

[Impeccable](https://impeccable.style/) gives coding agents practical UI design guidance, commands for building and refining interfaces, and optional automatic checks for common design issues.

#### Codex CLI setup

1. Run `npx impeccable install` from a terminal on this device; you do not need to run it from a project directory.
2. Choose **Codex** and select **all your projects** (the user-wide install) so Impeccable is available across your local projects.
3. Reload Codex. In each project where you want tailored design guidance, use `$impeccable init` in chat to record its product context.
4. To enable automatic checks, run `$impeccable hooks on`, then review and trust the hooks with `/hooks`.

#### Example prompts

| Goal | Example in Codex CLI |
| --- | --- |
| Build a new interface | `$impeccable create a signup page for a neighborhood book club. Make the reading schedule and join action easy to find.` |
| Improve an existing page | `$impeccable polish the pricing page. Keep the current colors and plan names, and make the differences easier to compare.` |
| Find design problems | `$impeccable critique the analytics dashboard. Focus on hierarchy, readability, and mobile use.` |
| Run a final quality pass | `$impeccable audit the checkout flow for accessibility, responsive behavior, and performance issues.` |
| Compare visual directions | `$atomic-visual-options Compare three layouts for this settings page. Show the differences before changing the implementation.` |

### Atomic Claude

[Atomic Claude](https://atomic.alonso.network/) combines repository wikis, a local code graph, workflow commands, agents, and a browser interface. Its Atomic CLI can be used from Codex for code indexing and browsing; its wiki-writing commands, hooks, and agent bundle are native to Claude Code.

#### Codex CLI setup

1. Install the Atomic CLI:

   ```sh
   curl -fsSL https://raw.githubusercontent.com/damusix/atomic-claude/main/install.sh | bash
   ```

2. Check the installation with `atomic doctor`.
3. In a repository, build the local symbol graph once with `atomic code index`.
4. Install the extracted Codex skills from this repository if you want the same workflow guidance in Codex:

   ```sh
   mkdir -p .agents/skills
   cp -R skills/atomic_claude/skills/* .agents/skills/
   ```

   Restart Codex to load the copied skills. For role prompts, ask Codex to use a file from `skills/atomic_claude/agents/` as delegated-agent instructions; these Markdown files are prompt templates, not auto-registered Codex agents.

#### Atomic CLI commands

Run these in the target repository:

| Task | Command |
| --- | --- |
| Initialize Atomic's repo configuration | `atomic repo init` |
| Build or rebuild the code graph | `atomic code index` |
| Sync the graph after code changes | `atomic code sync` |
| Explore a behavior or code path | `atomic code explore "how is a request authenticated?"` |
| Find callers of a symbol | `atomic code callers verifyToken` |
| Check a symbol's blast radius | `atomic code impact --depth 2 verifyToken` |
| Browse the repository locally | `atomic serve . --open` |
| Check or update the installed CLI | `atomic doctor` / `atomic update` |

`atomic serve` is read-only and serves the existing wiki and code graph on localhost. The repo wiki itself is generated through Atomic Claude's `/setup-wiki` and `/refresh-wiki` commands; Codex can read those generated files, but those slash commands require Claude Code.

#### Example prompts

| Goal | Example in Codex CLI |
| --- | --- |
| Locate a code path | `Use skills/atomic_claude/agents/atomic-investigator.md as the role prompt. Find where requests are authenticated and list the callers with file and line references.` |
| Diagnose a failure | `$atomic-debug The login test started failing after the middleware change. Form ranked hypotheses and test the cheapest one first.` |
| Implement a change test-first | `$atomic-tdd Add rate limiting to the login endpoint. Write a failing regression test before changing production code.` |
| Review a change | `Use skills/atomic_claude/agents/atomic-reviewer.md to review the current diff. Return only actionable findings with file and line references.` |
| Audit existing code | `Use skills/atomic_claude/agents/atomic-deslopper.md to inspect src/auth for dead code, unnecessary complexity, and convention drift. Do not edit files.` |
| Refresh the graph after edits | `Run atomic code sync, then use atomic code impact to check the callers of verifyToken.` |

#### Claude Code setup

When using Atomic Claude's native integration, install its Claude bundle with `atomic claude install`. In each repository's Claude Code session, run `/setup-wiki` once and `/refresh-wiki` to build or refresh the repository wiki. The Codex CLI setup above does not install this Claude-specific bundle.
