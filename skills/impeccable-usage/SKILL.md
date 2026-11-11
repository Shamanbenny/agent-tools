---
name: impeccable-usage
description: >
  Explain how to use or install Impeccable, prepare a repository for it, choose a
  design command, or understand PRODUCT.md, DESIGN.md, hooks, and workflow setup.
  Use for Impeccable usage and onboarding questions, including requests to apply
  that setup. For actual interface design, audit, polish, or redesign work, use
  the installed impeccable skill instead.
---

# Impeccable usage

Answer ordinary usage and setup questions from this guide. Do not browse upstream
to rediscover the command catalog. This companion is based on the installed
Impeccable 4.8.2 skill; it does not bundle its launcher, playbooks, or detector.
Use installed skill metadata/references for version-specific behavior, and consult
upstream only for a requested update or a gap local documentation cannot resolve.

## Installation and scope

`impeccable-usage` explains the tool; it does not install the `impeccable` skill.
Do not claim the main skill is available just because this companion is present.

To install the main tool, run `npx impeccable install`, select the coding agent,
and choose project-only or user-wide scope; it also offers the design hook where
the agent supports one. The prompts can be skipped with flags, for example
`npx impeccable install --providers=codex --scope=project|global --no-hooks`
(`--providers` accepts a comma-separated list such as `claude,codex,cursor`).
For project-only installation, start in the target repo; user-wide installation
makes the skill available across projects but does not create product/design
context for each repo. Reload the agent after installation. Use
`npx impeccable update` only when updating is requested. Installation requires
Node.js 18.19+ and network access.

To install this companion from an agent-tools checkout, copy only its directory
to a discovered skills directory, preserving any existing copy:

```sh
agent_tools_root=/absolute/path/to/agent-tools
destination="$PWD/.agents/skills"
# Or: destination="$HOME/.agents/skills" for all projects.
mkdir -p "$destination"
if [ -e "$destination/impeccable-usage" ] || [ -L "$destination/impeccable-usage" ]; then
  printf 'Skipped existing skill: impeccable-usage\n'
else
  cp -R "$agent_tools_root/skills/impeccable-usage" "$destination/impeccable-usage"
fi
```

Examples below use Codex chat syntax, `$impeccable <command>`. These are agent
requests, not shell commands. The shell launcher is a separate interface.

## Repository context

No particular framework, package manager, or directory layout is required.
Inspect existing guidance, actual app boundaries, and docs before recommending
new files. Do not create product truth from guesses or overwrite an existing
design system merely because a context file is missing.

| Artifact | Purpose | How it is created or maintained |
| --- | --- | --- |
| `PRODUCT.md` | Users, purpose, platform, capabilities, constraints, confirmed brand facts | `$impeccable init` interviews for missing facts; also updates existing product context |
| `DESIGN.md` | Visual system, tokens, typography, layout, components | `$impeccable document` records an incumbent system; new design work can establish a new world |
| Surface briefs | Task/surface-specific decisions and visitor mode | Managed by relevant design workflows; do not fabricate their schema |
| `.impeccable/config.json` | Shared workflow and detector settings | Use the relevant installed command |
| `.impeccable/config.local.json` | Private local overrides | Keep out of version control |
| `.impeccable/design.json` | Design sidecar for details beyond DESIGN.md's portable schema | Managed with the design documentation workflow |

`init` captures product truth; it does not create `DESIGN.md` or choose a visual
world. It includes a real confirmation round for a new product record. `document`
extracts the visual system from evidence and follows the installed playbook's
confirmation flow before updating an existing file. A coherent existing UI is
design evidence even without `DESIGN.md`.

Monorepos may inherit context or have app-specific records. Resolve the requested
app/target before creating competing root and child files. These documents can be
versioned when shared and factual; exclude private config, credentials, caches,
and runtime artifacts according to the installed tool and repo conventions.

## Recommend the next command

For usage questions, give advice without running design workflows. Choose by the
user's intent and current repo evidence; do not prescribe every command in order.

| Goal | Codex chat request |
| --- | --- |
| Capture or update product context | `$impeccable init` |
| Record the existing design system | `$impeccable document` |
| Plan UX/UI before implementation | `$impeccable shape <surface>` |
| Build a new surface | `$impeccable Create <surface and constraints>` |
| Review hierarchy and usability | `$impeccable critique <surface>` |
| Check accessibility, responsiveness, technical quality | `$impeccable audit <surface>` |
| Finish an existing implementation | `$impeccable polish <surface>` |
| Simplify | `$impeccable distill <surface>` |
| Increase or reduce visual intensity | `$impeccable bolder <surface>` / `$impeccable quieter <surface>` |
| Improve errors, localization, edge cases | `$impeccable harden <surface>` |
| Improve first-run and empty states | `$impeccable onboard <flow>` |
| Add motion, color, typography, layout, or personality | `$impeccable animate`, `colorize`, `typeset`, `layout`, or `delight` with a target |
| Improve copy, device adaptation, or performance | `$impeccable clarify`, `adapt`, or `optimize` with a target |
| Extract reusable design elements | `$impeccable extract <target>` |
| Pursue ambitious visual effects | `$impeccable overdrive <surface>` |
| Iterate in a live web app | `$impeccable live` |
| Generate web variants of a named element | `$impeccable generate [n] [action] [element]` (1 to 8 variants, default 3) |
| Inspect/repair artifact drift | `$impeccable doctor` |
| Add or remove a standalone command shortcut | `$impeccable pin <command>` / `$impeccable unpin <command>` |

`teach` aliases `init`; `craft` is deprecated in favor of an ordinary new-work
request. A bare `$impeccable` presents context-aware suggestions, not automatic
implementation. `live`, `generate`, and the HTML/CSS detector are web-only; do not
recommend them for native iOS or Android code. Native design work uses the main
skill's platform playbooks.

For an existing app, a useful path is init if product context is needed, document
if the incumbent system should be recorded, then a scoped critique or refinement.
For a new app, capture product truth and request the surface to build; do not
require documenting a design system that does not exist yet. Narrow refinements
can proceed from incumbent evidence without forcing a full onboarding interview.

## Hooks and maintenance

Hooks are optional. `$impeccable hooks on` enables the project design detector;
on Codex the user must review/trust it through `/hooks` the first time. Use
`$impeccable hooks status` to inspect and `$impeccable hooks off` to disable;
`hooks reset` removes the config and every hook entry `on` installed. Detector
exceptions use `hooks ignore-value`, `ignore-rule`, or `ignore-file`; load the
main skill's `reference/hooks.md` first, and ask the user before `ignore-rule`
or `ignore-file`. Do not hand-edit hook plumbing or suppress real design issues
as setup work. The hook never blocks an edit on Codex or Claude Code; it reports
after the edit. Older Cursor projects on the legacy `preToolUse` gate can block
until `hooks on`, `impeccable update`, or `doctor --fix` migrates them.

Security facts to state when asked about trust: the hook runs the detector
engine unsandboxed with the agent's environment and network access, and the
first edit can download the engine (checked against the skill's committed
digests). Live Mode's Apply step, when no chat agent has polled in the last
minute, can launch `codex` or `claude` with approvals and sandboxing off; set
`IMPECCABLE_LIVE_COPY_AGENT=chat` to keep Apply in chat, or `off` to disable it.

`$impeccable doctor` concerns artifact/schema drift, not updating the installed
tool or redesigning a page. `init` updates product truth; `document` updates visual
truth. A commit count alone does not prove either document is inaccurate.

## When execution is requested

Use the installed `impeccable` skill and its relevant playbook. Resolve the
launcher relative to the actual skill directory, not the target repo or this
companion:

```text
<installed-impeccable-skill-dir>/scripts/impeccable context [--target <path>]
```

Keep cwd at the target project. Context runs once per design session and resolves
the product/design records and surface/platform guidance; use `impeccable.cmd` on
Windows without `sh`. The launcher may download its bundled binary on first use.
If unavailable, disclose that context loading did not run, read existing context
directly, and continue through permitted tools. Do not run it merely to answer a
generic usage question.

The main skill's craft floor is required before UI edits, and rendered inspection
at relevant viewport sizes is part of verification. Refinement preserves the
incumbent identity and scope; redesign requires an explicit replacement brief.
Do not enable hooks, initialize product docs, or change a design as a side effect
of answering a usage question.

Sources for intentional refreshes: [Impeccable](https://impeccable.style/),
[workflow documentation](https://impeccable.style/docs/), and the installed
`impeccable/SKILL.md` plus `reference/routing.md`, `init.md`, `document.md`,
`hooks.md`, and `doctor.md`. Resolve installed references dynamically; this
companion remains usable when copied by itself.
