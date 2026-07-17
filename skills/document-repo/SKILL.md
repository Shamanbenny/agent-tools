---
name: document-repo
description: Review and maintain documentation in the current repository after code or configuration changes. Use for repository-wide documentation audits, updating docs to match the current context, removing stale or inaccurate material, and deciding whether new documentation is needed.
---

# Document Repo

Maintain repository documentation as part of the current task. Work in three phases and preserve the repository's existing documentation style.

## Scope and discovery

Start by understanding the current task, the changes already made or requested, and the repository's conventions. Inspect relevant diffs, source/configuration files, routes or public interfaces, and existing documentation before editing.

Search tracked and untracked repository files for documentation, including:

- Markdown: `README.md`, `*.md`, `*.mdx`
- Plain text: `*.txt`, `*.rst`, `*.adoc`
- Structured documentation: `*.yaml`, `*.yml`, `*.json`, and `*.toml` when they contain human-facing descriptions, examples, or configuration guidance
- Documentation-like files in known locations such as `docs/`, `documentation/`, `guides/`, `examples/`, and package-level READMEs

Do not treat generated output, dependency directories, caches, binaries, lockfiles, or vendored third-party content as documentation targets unless the user explicitly includes them. Use `rg --files` and `rg` for discovery. Check repository status first and avoid overwriting unrelated user changes.

## Phase 1: reflect the current changes

Update existing documentation that is affected by the current context. Prefer the smallest accurate edit, and preserve established terminology, structure, links, examples, and voice.

Check for at least:

- changed commands, setup steps, environment variables, routes, APIs, file paths, or configuration keys
- changed behavior, defaults, permissions, error handling, or user-visible workflows
- examples that no longer run or describe the old interface
- links, headings, tables of contents, and references made incorrect by the changes

Document durable behavior and usage, not implementation trivia. Do not add a changelog entry or timestamped history section unless the repository already uses a changelog format for that information.

## Phase 2: remove stale or inaccurate material

Audit the documentation you found for contradictions, obsolete instructions, duplicate guidance, dead links, references to removed features, and claims unsupported by the current repository. Remove or rewrite stale material rather than preserving it as historical narrative.

Before deleting a whole document or substantial section, confirm that it is genuinely obsolete and not a source of unique required information. If it contains useful current material mixed with stale content, keep the useful parts and edit surgically. Do not remove documentation merely because it is brief, old-looking, or unrelated to the current change.

Review the resulting diff for accidental scope expansion, broken Markdown, inconsistent names, and unsupported claims. Run the repository's relevant documentation checks or existing validation commands when available.

## Phase 3: decide whether new documentation is needed

After phases 1 and 2, assess whether the current change leaves an important user, contributor, operator, or maintainer question unanswered. New documentation may be warranted when a workflow is newly introduced, a public interface is difficult to discover, a breaking change needs migration guidance, or a non-obvious operational constraint has no suitable home.

Do not create new documentation automatically. When you believe new documentation is needed, use the product's ask-question tool to present the user with:

1. the documentation gap and evidence for it
2. the proposed document or section and its intended audience
3. where it would live and what it would cover
4. the consequence of deferring it

Wait for the user's decision before adding new documentation. If they approve, create only the smallest useful document or section and follow local conventions. If they decline, finish the existing-document updates and report the deferred gap. If the ask-question tool is unavailable, ask the user directly in the final response and do not create the new documentation without approval.

## Completion checklist

- Existing relevant documentation reflects the current task and repository behavior.
- Stale or inaccurate instructions were removed or corrected.
- No default changelog-style entry was introduced.
- New documentation was added only after explicit user approval.
- The final diff contains only in-scope documentation changes, and relevant checks were run or their absence is noted.
