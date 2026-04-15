---
name: llm-wiki-query
description: Workflow for querying the LLM Wiki.
---

# LLM Wiki Query Workflow

Query the wiki for a specific topic, synthesize results, and suggest next steps.

## Configuration Check

Resolve `{{vaultPath}}` the command `obsidian vault info=path`.

## Inputs

- Query topic or question
- Optional focus (e.g. "methods", "results", "thesis relevance")
- Optional action (e.g. "write an introduction into this topic for my thesis")

## Steps

### Step 1: Query wiki for relevant content

- Run the "LLM Wiki Searcher" subagent to read the current wiki content relevant to the source's topics.
  - Dead simple prompt: `Search the wiki for content relevant to <topics> and respond with a summary`
  - Keep the prompt simple.

### Step 2: Attend to the user's request

- With the response from the searcher, attend to the user's specific query or requested action.
  - If the user requested a specific action, execute that action with the search results as context.
  - If the user requested a general query, provide an answer with the appropriate level of detail.
  - Cite relevant wiki pages in the response

### Step 3: Suggest next steps (optional)

The subagent may recommend creating a synthesis page when an answer is valuable and no related syntheses were found. If so, suggest a title and one-line description for the synthesis, and ask the user if they want to create it. If so:

- Delegate the synthesis to the "LLM Wiki Synthesizer" subagent.
  - Forward the outcome of the wiki search as context for the synthesis.
  - Prompt structure: `Create a synthesis page for <synthesis-title>. Relevant context: <wiki-search-results>. Relevant cited pages: <cited-pages>.`
