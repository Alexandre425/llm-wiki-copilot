# LLM Wiki Setup Workflow

Initialize or validate the LLM Wiki layer inside the user's vault without overwriting existing content.

## Configuration Check

Resolve `{{vaultPath}}` the command `obsidian vault info=path`.

## Steps

### Step 1: Validate base directories

Ensure these paths exist:
```bash
# no trailing slash!
obsidian folder path="03-Resources/Raw-Sources"
obsidian folder path="03-Resources/LLM-Wiki"
obsidian folder path="03-Resources/LLM-Wiki/sources"
obsidian folder path="03-Resources/LLM-Wiki/concepts"
obsidian folder path="03-Resources/LLM-Wiki/entities"
obsidian folder path="03-Resources/LLM-Wiki/syntheses"
```

Create missing directories only.

### Step 2: Initialize core wiki files

If missing, create these files from templates:
- `schema.md` from `templates/schema.md`
- `index.md` from `templates/index.md`
- `log.md` from `templates/log.md`

Do not overwrite existing files unless the user explicitly asks for reset.

### Step 3: Add setup log entry

Append to `log.md` if setup created or changed anything:
- `## [YYYY-MM-DD] setup | initialized llm wiki structure`

### Step 4: Confirm status

Report:
- Created directories
- Created files
- Existing files kept intact
- Recommended next action: ingest first source

## Output Format

```
LLM WIKI SETUP COMPLETE

Created directories:
- ...

Created files:
- ...

Preserved existing files:
- ...

Next step:
- Add a source to 03-Resources/Raw-Sources/ and run wiki ingest.
```
