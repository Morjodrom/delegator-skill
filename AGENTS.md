# Repository Guidelines

## Project Structure & Module Organization

This repository contains the `delegator` Codex skill. Its source is intentionally small and documentation-first:

- `skill/SKILL.md` is the canonical skill definition, including the delegation workflow and agent contract.
- `skill/agents/openai.yaml` supplies the Codex-facing display name and default prompt.
- `skill/references/` holds optional supporting guidance. For example, `efficiency-audit.md` is read only for token-usage or orchestration retrospectives.

Keep workflow rules in `SKILL.md`; put detailed, conditional material in a focused file under `skill/references/` and link to it from the skill.

## Build, Test, and Development Commands

There is no build system or automated test suite in this repository. Before submitting changes, use focused manual checks:

```sh
cat skill/SKILL.md                 # review rendered source and Markdown structure
git diff --check                   # catch whitespace errors
git diff -- AGENTS.md skill/       # review the complete proposed change
```

Validate YAML edits by checking indentation and preserving the existing top-level `interface` and `policy` keys. Do not add generated files or local IDE settings.

## Coding Style & Naming Conventions

Write concise Markdown with sentence-style headings, short paragraphs, and imperative instructions. Use fenced code blocks for command examples and relative Markdown links for repository files. Keep the YAML in `skill/agents/openai.yaml` at two-space indentation; quote user-facing strings when they contain punctuation. Name reference files in lowercase kebab-case, such as `efficiency-audit.md`.

When changing delegation guidance, preserve its explicit-only behavior and keep rules actionable: state the trigger, scope, constraints, validation, and expected return information.

## Testing Guidelines

Review changes as a contributor would: verify links resolve, examples match the documented commands, and new instructions do not conflict with `skill/SKILL.md`. For changes to the skill workflow, exercise the relevant decision path mentally (no delegation for trivial edits; disjoint ownership before parallel work). Add or update a reference only when it supports a clearly conditional workflow.

## Commit & Pull Request Guidelines

Recent commits use short imperative subjects, e.g. `prepare for github` and `separate the meta files from the skill itself`. Follow that pattern: lowercase, concise, and focused on one change.

Pull requests should explain the contributor-facing behavior changed, list affected paths, and include the manual validation performed. Link the related issue when one exists. Keep unrelated formatting and IDE changes out of the diff.
