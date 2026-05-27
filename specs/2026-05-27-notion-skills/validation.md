# Validation: Notion Skills Integration

## Acceptance Criteria

### CRUD Operations
- [ ] Create page with explicit parent page ID succeeds
- [ ] Create page with explicit database ID succeeds
- [ ] Retrieve page by ID returns correct content
- [ ] Update page by ID applies changes
- [ ] Query database by database ID returns results

### No Search Dependency
- [ ] Zero calls to search-by-title API in skill code
- [ ] All operations use only explicit IDs from configuration
- [ ] Skills fail gracefully with clear error when ID is missing/invalid

### Missing Configuration
- [ ] `notion` skill fails with clear error when both `NOTION_PAGES` and `NOTION_DATABASES` are unset
- [ ] `notion-todo` skill fails with clear error when `NOTION_DATABASES` is unset or `todo` entry missing
- [ ] Error messages explain what env var is needed and how to set it

### Todo Integration
- [ ] Notion-backed todo skill can add items to configured database
- [ ] Notion-backed todo skill can list items from configured database
- [ ] Notion-backed todo skill can mark items complete
- [ ] Existing todo skill continues to work unchanged

## How to Test

1. Configure a Notion page ID and database ID in env vars
2. Run each CRUD operation and verify success
3. Run the Notion-backed todo skill end-to-end
4. Verify existing todo skill still works
5. Unset both env vars and run `notion` skill — verify clear error message explaining what to configure
6. Unset `NOTION_DATABASES` and run `notion-todo` — verify clear error about missing `databases.todo`
7. Set an invalid ID and run an operation — verify graceful failure with suggestion to reconfigure
