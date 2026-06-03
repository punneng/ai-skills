---
name: notion-todo
description: Manage a todo list stored in Notion using explicit database IDs via Notion MCP.
trigger: /notion-todo
---

# /notion-todo — Notion-backed Todo

Manage a todo list stored in a Notion database. Uses the `notion` skill's configuration (`NOTION_DATABASES` env var) to resolve the todo database by name.

## When to use

- User says: "add a todo", "list my todos", "complete todo", "delete todo"
- User invokes `/notion-todo`
- Working with a todo list backed by Notion

## Configuration

Requires `NOTION_DATABASES` env var with a `todo` entry:

```bash
NOTION_DATABASES="todo:x9y8z7w6v5u4..."
```

The skill resolves `databases.todo` to the actual database ID.

## Database Schema

The Notion database should have these properties:

| Property | Type | Purpose |
|----------|------|---------|
| `Name` | title | Todo item text |
| `Status` | select | `todo`, `in_progress`, `done` |

If `Status` is missing, the skill should prompt the user to add it or create it via `notion_API-update-a-data-source`.

## Flow

### Step 1 — Resolve database

Read `NOTION_DATABASES` from environment. Parse and look up `todo` entry. If not found, prompt the user to configure `databases.todo`.

### Step 1.5 — Validate database ID (first use only)

On first invocation, validate the database ID:

1. Call `notion_API-retrieve-a-database` with the `databases.todo` ID
2. If it returns an error, stop with: "Cannot access database `databases.todo`. Check your `NOTION_DATABASES` env var."
3. Cache validation result for the session

### Step 2 — Execute operation

#### Add todo

Use `notion_API-post-page` with:
- `parent`: `{ "database_id": "<resolved-todo-db-id>" }`
- `properties`:
  ```json
  {
    "Name": { "title": [{ "text": { "content": "<todo text>" } }] },
    "Status": { "select": { "name": "todo" } }
  }
  ```

#### List todos

Use `notion_API-query-data-source` with:
- `data_source_id`: the configured database ID (`databases.todo` IS the `data_source_id`)
- `filter`: optional, filter by status (e.g., `{ "property": "Status", "select": { "equals": "todo" } }`)
- `sorts`: omit — use default order (Notion returns creation order)

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
- `properties`: fields to update (Name, Status, Due, etc.)

#### Delete todo

Use `notion_API-patch-page` with:
- `page_id`: the todo page ID
- `archived`: true

### Step 3 — Confirm

Report the result:
- Add: "Added todo: `<text>`"
- List: "Showing N todos: X todo, Y in_progress, Z done"
- Complete: "Marked done: `<text>`"
- Delete: "Deleted: `<text>`"

## Behavior rules

1. **Never use search-by-title** — always resolve `databases.todo` from env vars
2. **Fail gracefully** — clear error if `NOTION_DATABASES` is unset or `todo` entry missing
3. **Support partial matching** — when user references a todo by number or partial text, match against the list results
4. **Never log raw IDs** — print only todo text and status
