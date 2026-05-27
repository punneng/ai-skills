---
name: notion
description: Create and manage Notion content using explicit page/database IDs via Notion MCP.
trigger: /notion
---

# /notion — Notion Integration

Create, read, update, and query Notion pages and databases using explicit IDs from environment variables. Bypasses the unreliable search-by-title API.

## When to use

- User says: "create a notion page", "add to notion", "query my notion database", "update notion"
- User invokes `/notion`
- Working with Notion content where page/database IDs are pre-configured

## Configuration

This skill reads Notion page and database IDs from environment variables.

### Environment Variables

```bash
NOTION_PAGES="workspace:a1b2c3d4e5f6...,home:d4e5f6g7h8i9..."
NOTION_DATABASES="todo:x9y8z7w6v5u4...,notes:m1n2o3p4q5r6..."
```

Format: comma-separated `name:id` pairs.

### Named References

Skills reference destinations by name:

- `pages.workspace` → ID from `NOTION_PAGES` entry named `workspace`
- `databases.todo` → ID from `NOTION_DATABASES` entry named `todo`

## Flow

### Step 1 — Parse configuration

Read `NOTION_PAGES` and `NOTION_DATABASES` from environment. Parse into lookup tables:

```
pages = { "workspace": "a1b2c3d4...", "home": "d4e5f6g7..." }
databases = { "todo": "x9y8z7w6...", "notes": "m1n2o3p4..." }
```

**Validation:**
- If **both** are unset → stop with error: "Set `NOTION_PAGES` or `NOTION_DATABASES` in your environment. Example: `NOTION_PAGES=\"workspace:id\"`"
- If **only one** is set → proceed, but only operations for the configured type will work
- If a referenced name is not found → list available names from the configured type

### Step 1.5 — Validate IDs (first use only)

On first invocation, validate each configured ID:

1. For each page ID, call `notion_API-retrieve-a-page` with the ID
2. For each database ID, call `notion_API-retrieve-a-database` with the ID
3. Extract `data_sources[0].id` from the database response and cache it as the `data_source_id` for later queries
4. Report any IDs that return errors (likely invalid or lacking permissions)
5. Cache validation result for the session — skip on subsequent calls

### Step 2 — Resolve named reference

When user references `pages.<name>` or `databases.<name>`:

1. Look up `<name>` in the corresponding table
2. If found, use the ID for Notion MCP calls
3. If not found, list available names and ask user to choose

### Step 3 — Execute operation

#### Create page

Use `notion_API-post-page` with:
- `parent`: `{ "page_id": "<resolved-id>" }` or `{ "database_id": "<resolved-id>" }`
- `properties`: page properties (title, etc.)
- `children`: optional initial content blocks

#### Retrieve page

Use `notion_API-retrieve-a-page` with:
- `page_id`: resolved page ID

#### Update page

Use `notion_API-patch-page` with:
- `page_id`: resolved page ID
- `properties`: properties to update
- `archived`: true to delete, false to restore

#### Query database

First, resolve the `data_source_id` from the configured `database_id`:
1. Call `notion_API-retrieve-a-database` with the compact `database_id`
2. Extract `data_sources[0].id` → this is the `data_source_id`

Then use `notion_API-query-data-source` with:
- `data_source_id`: the resolved UUID from `data_sources`
- `filter`: optional filter conditions
- `sorts`: optional sort configuration
- `page_size`: number of results (default 100)

#### Get database schema

Use `notion_API-retrieve-a-data-source` with:
- `data_source_id`: resolved database ID

### Step 4 — Confirm

Report the result using only the named reference (e.g., "Created page in `pages.workspace`"). Never print raw IDs.

## Behavior rules

1. **Never use search-by-title** — all operations use explicit IDs from env vars
2. **Validate on startup** — check env vars exist before any operation
3. **Use named references** — resolve `pages.<name>` or `databases.<name>` to IDs
4. **Support multiple destinations** — env vars can contain any number of entries
5. **Fail gracefully** — clear error when ID is missing or invalid
6. **Never log raw IDs** — print only name references in output

## How to obtain Notion IDs

### From a URL

URL: `https://notion.so/workspace/Page-Name-a1b2c3d4e5f6...`
ID: last segment after final hyphen (32 chars, may contain hyphens)

### From the UI

1. Open page/database
2. Click "Share" → "Copy link"
3. Extract ID from URL

### Initial discovery

Use `GET /v1/search` once to find IDs, then set them in `.env`. No further search calls needed.
