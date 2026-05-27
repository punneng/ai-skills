# Plan: Notion Skills Integration

## Phase: Create Notion-aware skills alongside existing todo skill

### 1. Setup & Configuration
- [x] Create `skills/in-progress/` bucket with `README.md`
- [x] Create Notion skill under `skills/in-progress/notion/`
- [x] Define env var schema: `NOTION_PAGES` and `NOTION_DATABASES` as comma-separated `name:id` pairs
- [x] Support named references so skills can look up destinations by name instead of raw IDs
- [x] Document how to obtain Notion page/database IDs from URLs

### 2. Core Skill Implementation
- [x] Build base Notion skill that uses MCP with explicit ID resolution
- [x] Implement page creation with parent page/database ID (by ID or by named reference)
- [x] Implement page retrieval by ID or named reference
- [x] Implement page update by ID or named reference
- [x] Implement database query by database ID or named reference
- [x] Implement configuration resolver that maps named references to actual IDs
- [x] Validate that all configured page/database IDs exist on startup

### 3. Todo Skill Extension
- [x] Create Notion-backed todo skill (`notion-todo`) in `skills/in-progress/`
- [x] Configure todo skill to reference `databases.todo` named database
- [x] Implement todo operations (add, list, complete, update, delete) via Notion MCP with named references

### 4. Verification
- [ ] Test CRUD operations with multiple pages and databases configured
- [ ] Test named reference resolution for both pages and databases
- [ ] Verify skills work end-to-end without search-by-title
- [ ] Test configuration validation when IDs are missing or invalid
- [ ] Move skill to published bucket (e.g., `integrations/`) and add to top-level `README.md`
- [ ] Document configuration requirements for users
