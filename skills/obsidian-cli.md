---
# From https://github.com/kepano/obsidian-skills/blob/main/skills/obsidian-cli/SKILL.md
name: obsidian-cli
description: Interact with Obsidian vaults using the Obsidian CLI to read, create, search, and manage notes, tasks, properties, and more. Use when the user asks to interact with their Obsidian vault, manage notes, search vault content or perform vault operations from the command line.
---

# Obsidian CLI

Use the `obsidian` CLI to interact with a running Obsidian instance. Requires Obsidian to be open.

If Obsidian is not running or CLI commands fail, report that explicitly and fall back to direct filesystem operations.

## Command reference

Run `obsidian help` to see all available commands. This is always up to date.

## Syntax

**Parameters** take a value with `=`. Quote values with spaces:

```bash
obsidian create name="My Note" content="Hello world"
```

**Flags** are boolean switches with no value:

```bash
obsidian create name="My Note" silent overwrite
```

For multiline content use `\n` for newline and `\t` for tab.

## File targeting

Many commands accept `file` or `path` to target a file. Without either, the active file is used.

- `file=<name>` — resolves like a wikilink (name only, no path or extension needed)
- `path=<path>` — exact path from vault root, e.g. `folder/note.md`

## Vault targeting

Commands target the most recently focused vault by default. Use `vault=<name>` as the first parameter to target a specific vault:

```bash
obsidian vault="My Vault" search query="test"
```

## Common patterns

```bash
obsidian read file="My Note"
obsidian create name="New Note" content="# Hello" template="Template" silent
obsidian append file="My Note" content="New line"
obsidian search query="search term" limit=10
obsidian daily:read
obsidian daily:append content="- [ ] New task"
obsidian property:set name="status" value="done" file="My Note"
obsidian tasks daily todo
obsidian tags sort=count counts
obsidian backlinks file="My Note"
obsidian links file="My Note"
# helpful for wiki maintenance
obsidian deadends
obsidian orphans
obsidian unresolved
```

Use `silent` to prevent files from opening. Use `total` on list commands to get a count.

## LLM Wiki recipes

Use these commands when operating under `03-Resources/LLM-Wiki/`.

Before creating a new wiki page, read the matching template file from the llm-wiki skill directory and copy its structure into the note content you pass to `obsidian create`.

When the vault already has a matching template file in `templates/`, prefer `obsidian create ... template="<template-file>"` so Obsidian applies the template directly.

- Source notes should follow `templates/source-page.md`
- Index updates should follow `templates/index.md`
- Log entries should follow `templates/log.md`
- Synthesis pages should follow `templates/synthesis-page.md`

Useful discovery commands:

```bash
obsidian files folder="templates"
obsidian read path="templates/source-page.md"
obsidian read path="templates/index.md"
obsidian read path="templates/log.md"
obsidian read path="templates/synthesis-page.md"
```

```bash
# Discover relevant pages before editing
obsidian search query="roofline" path="03-Resources/LLM-Wiki" format=json
obsidian search:context query="cache-aware" path="03-Resources/LLM-Wiki" format=json

# Read core files
obsidian read path="03-Resources/LLM-Wiki/schema.md"
obsidian read path="03-Resources/LLM-Wiki/index.md"
obsidian read path="03-Resources/LLM-Wiki/log.md"

# Create/update wiki pages
obsidian create path="03-Resources/LLM-Wiki/sources/example-source.md" content="# Example"
obsidian create path="03-Resources/LLM-Wiki/sources/example-source.md" template="source-page.md"
obsidian append path="03-Resources/LLM-Wiki/log.md" content="\n## [2026-04-07] ingest | Example\n..."

# Verify page shape after creation
obsidian outline path="03-Resources/LLM-Wiki/sources/example-source.md" format=tree
obsidian properties path="03-Resources/LLM-Wiki/sources/example-source.md" format=yaml

# Structural checks used by lint workflows
obsidian orphans
obsidian deadends
obsidian unresolved
obsidian links path="03-Resources/LLM-Wiki/sources/example-source.md"
obsidian backlinks path="03-Resources/LLM-Wiki/sources/example-source.md" counts

# Metadata operations
obsidian properties path="03-Resources/LLM-Wiki/sources/example-source.md" format=yaml
obsidian property:set path="03-Resources/LLM-Wiki/sources/example-source.md" name="status" value="active"
```

## Suggested command order for wiki tasks

1. `obsidian help` (or `obsidian help <command>`) to confirm exact syntax.
2. `obsidian search` or `obsidian search:context` to find existing relevant pages.
3. `obsidian read` for candidate pages.
4. `obsidian create` or `obsidian append` for updates.
5. `obsidian links`/`obsidian backlinks`/`obsidian orphans` as a quick integrity check.

### Additional developer commands

Run JavaScript in the app context:

```bash
obsidian eval code="app.vault.getFiles().length"
```

Run `obsidian help` to see additional developer commands including CDP and debugger controls.
