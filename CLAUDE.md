# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A **Claude Code plugin marketplace** named `leverate` that ships a single plugin (`lev-market`) containing user-level skills. There is no build system, no tests, and no application runtime — the deliverable is a set of static config files and Markdown skills that Claude Code loads.

## Layout

- `.claude-plugin/marketplace.json` — declares the marketplace and lists its plugins. Each plugin entry has a `name` and a `source` (relative path to the plugin directory).
- `plugins/<plugin-name>/.claude-plugin/plugin.json` — per-plugin metadata: `name`, `version`, `description`. The `name` here must match the entry in `marketplace.json`.
- `plugins/<plugin-name>/skills/<skill-name>/SKILL.md` — one folder per skill. The folder name becomes the skill identifier; the file must be named `SKILL.md`.

## SKILL.md format

Each skill is a Markdown file with YAML frontmatter:

```markdown
---
name: "<skill-name>"
description: "Use when ... <trigger conditions>. <one-line summary>."
---

# Purpose / Behavior / Output / etc.
<body that tells Claude what to do>
```

Conventions observed in the existing skills:
- `description` starts with "Use when ..." and enumerates trigger phrases (including any `/<slash-command>` form). This text is what Claude reads to decide whether to invoke the skill, so make triggers explicit.
- Bodies are short and prescriptive — "Pick one X at random. Reply with exactly Y. No preamble." Style is more rules than prose.
- The `name` in frontmatter should match the skill's folder name.

## Common tasks

**Add a skill:** create `plugins/lev-market/skills/<name>/SKILL.md`, then bump `version` in `plugins/lev-market/.claude-plugin/plugin.json` so consumers pick up the change.

**Rename a skill:** rename the folder, update `name` in the SKILL.md frontmatter, and bump the plugin version.

**Add a new plugin:** create `plugins/<name>/` with `.claude-plugin/plugin.json` and a `skills/` subtree, then add a corresponding entry to `.claude-plugin/marketplace.json`.

There are no lint, test, or build commands — verification is "the JSON parses and the YAML frontmatter is well-formed."
