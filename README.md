# LLM Wiki Copilot

> [!WARNING]
> This implementation is work in progress.

Implementation of [Karpathy's LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) as a multi-agent system. For use with VSCode Copilot and Obsidian.

## Why Multi-Agent?

By giving each agent a narrow, well-defined role, the overall workflow is adhered to much more closely. Drift is minimized, and it allows the use of much faster models.

## Architecture

### Core Components

**Master Orchestrator**
- Delegates operations to specialized agents based on user intent

**Specialized Agents**
- Each handles a specific aspect of the workflow (extraction, summarization, writing, searching, etc.)

**Skills**
- Each workflow (setup, ingest, query, lint) is described as a skill that the orchestrator loads as required.

## Requirements

### System & Environment

- **Obsidian** running with the CLI plugin
- **VS Code** with Copilot extension (for agent framework)
- **Bash** for orchestration and shell commands
- **`pdftext`** for PDF text extraction

## Missing
- Template files for wiki setup (schema.md, index.md, log.md)
- Lint workflow