---
name: notion-todo
description: Manage a todo list stored in a Notion data source via Notion MCP.
trigger: /notion-todo
---

# /notion-todo — Notion-backed Todo

Manage a todo list stored in a Notion data source. Uses `MCP_NOTION_DATA_SOURCES_IDS` env var to resolve the data source by position (first entry is the todo list).

## When to use

- User says: "add a todo", "list my todos", "complete todo", "delete todo"
- User invokes `/notion-todo`
- Working with a todo list backed by Notion

## Configuration

Requires `MCP_NOTION_DATA_SOURCES_IDS` env var with comma-separated data source IDs:

```bash
MCP_NOTION_DATA_SOURCES_IDS="x9y8z7w6v5u4...,a1b2c3d4e5f6..."
```

The first entry is used as the todo data source.

## Data Source Schema

The Notion data source should have these properties:

| Property | Type | Purpose |
|----------|------|---------|
| `Name` | title | Todo item text |
| `Status` | select | `todo`, `in_progress`, `done` |

If `Status` is missing, prompt the user to add it or create it via `notion_API-update-a-data-source`.

## Flow

### Step 1 — Resolve data source

Read `MCP_NOTION_DATA_SOURCES_IDS` from environment. Split by comma, take the first entry as the todo data source ID. If the env var is unset or the list is empty, prompt the user to configure `MCP_NOTION_DATA_SOURCES_IDS`.

### Step 1.5 — Validate data source ID (first use only)

On first invocation, validate the data source ID:

1. Call `notion_API-retrieve-a-data-source` with the resolved ID
2. If it returns an error, stop with: "Cannot access data source `<id>`. Check your `MCP_NOTION_DATA_SOURCES_IDS` env var."
3. Cache validation result for the session

### Step 2 — Execute operation

#### Add todo

Use `notion_API-post-page` with:
- `parent`: `{ "type": "data_source_id", "data_source_id": "<resolved-data-source-id>" }`
- `properties`:
  ```json
  {
    "Name": { "title": [{ "text": { "content": "<todo text>" } }] },
    "Status": { "select": { "name": "todo" } }
  }
  ```

#### List todos

Use `notion_API-query-data-source` with:
- `data_source_id`: the resolved data source ID
- `filter`: optional, filter by status (e.g., `{ "property": "Status", "select": { "equals": "todo" } }`)

Display results as a numbered list with status indicators.

#### Complete todo

Use `notion_API-patch-page` with:
- `page_id`: the todo page ID (from list or user reference)
- `properties`:
  ```json
  {
    "Status": { "select": { "name": "done" } }
  }
  ```

#### Update todo

Use `notion_API-patch-page` with:
- `page_id`: the todo page ID
- `properties`: fields to update (Name, Status, etc.)

#### Delete todo

Use `notion_API-patch-page` with:
- `page_id`: the todo page ID
- `in_trash`: true

### Step 3 — Confirm

Report the result:
- Add: "Added todo: `<text>`"
- List: "Showing N todos: X todo, Y in_progress, Z done"
- Complete: "Marked done: `<text>`"
- Delete: "Deleted: `<text>`"

## Behavior rules

1. **Never use search-by-title** — always resolve data source ID from `MCP_NOTION_DATA_SOURCES_IDS`
2. **Fail gracefully** — clear error if `MCP_NOTION_DATA_SOURCES_IDS` is unset or empty
3. **Support partial matching** — when user references a todo by number or partial text, match against the list results
4. **Never log raw IDs** — print only todo text and status
