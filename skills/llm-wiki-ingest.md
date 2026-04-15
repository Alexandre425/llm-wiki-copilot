---
name: llm-wiki-ingest
description: Workflow for ingesting a new source into the LLM Wiki, including content extraction, summary, wiki updates, index and log updates.
---

# LLM Wiki Ingest Workflow

Process one source into the persistent wiki and update related pages, index, and log.

## Configuration Check

Resolve `{{vaultPath}}` the command `obsidian vault info=path`.

## Inputs

- Source path or source title
- Optional focus (for example: "methods", "results", "thesis relevance")

## Steps


### Step 0: Create staging area and artifact paths

- Create `staging-area/` in the active repository.
  - If it already exists, clear its contents.
- Generate a short source slug and define artifact paths inside `staging-area/`:
  - `<slug>.raw.txt`
  - `<slug>.summary.md`
  - `<slug>.wiki-search.md`
  - `<slug>.manifest.md`
- Keep prompts and responses between subagents dead simple and file-oriented.

### Step 1: Locate and validate source

- Locate the source based on the provided instructions.
- If the source is not in `{{vaultPath}}/03-Resources/Raw-Sources/`, move it there.
- Never modify source files.

### Steps 2: Extract source content and brief summary

- Run the "Document Extractor" subagent to extract the content of the source.
  - Prompt must be dead simple: `Extract the content of the source at <path> to <extracted-content-path>.`
  - The agent will return the path to the extracted content and a brief summary.

### Steps 3 and 4: Full summary and wiki query in parallel

Now that you know the topics addressed by the source, run two sub-agents in parallel:
- Run the "Document Summarizer" subagent to read the extracted content and create a detailed summary.
  - Dead simple prompt: `Summarize the content of the source at <extracted-content-path> and write the summary to <summary-file-path>.`
- Run the "LLM Wiki Searcher" subagent to read the current wiki content relevant to the source's topics.
  - Dead simple prompt: `Search the wiki for content relevant to <topics> and write results to <wiki-search-file-path>.`
  - Keep the prompt simple.

### Step 5: Create source page and update related pages

Delegate this step to the "LLM Wiki Writer" subagent.

- Dead simple prompt: `Create and/or update wiki pages for source <source-title> using <summary-file-path> and <wiki-search-file-path>. Write the manifest to <manifest-file-path>.`
- The writer creates or updates the source page and any impacted related pages.

### Step 6: Update index and log changes

Delegate this step to the "LLM Wiki Indexer" and "LLM Wiki Logger" subagents in parallel.

- Run the "LLM Wiki Indexer" subagent to update the wiki index from the manifest.
  - Dead simple prompt: `Update the index using manifest <manifest-file-path>.`

- Run the "LLM Wiki Logger" subagent to log changes from the manifest.
  - Dead simple prompt: `Update the log using manifest <manifest-file-path>.`

## Output Format

```
WIKI INGEST COMPLETE

Source:
- <title>

Pages created:
- ...

Pages updated:
- ...

Key synthesis changes:
- ...

Contradictions or uncertainties:
- ...

Suggested next ingestion/query:
- ...
```
