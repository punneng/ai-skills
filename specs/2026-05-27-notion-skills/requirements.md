# Requirements: Notion Skills Integration

## Scope

This feature creates skills that integrate with Notion using explicit page and database IDs, bypassing the unreliable search-by-title API from Notion MCP. The skills will extend alongside the existing todo skill, providing Notion-backed alternatives.

**In scope:**
- Base Notion skill with env var-based ID configuration
- CRUD operations via Notion MCP using page/database IDs
- Notion-backed todo skill variant
- Documentation for obtaining and configuring Notion IDs

**Out of scope:**
- Improving or fixing Notion MCP search-by-title
- Migrating or replacing the existing todo skill
- Direct Notion REST API calls (MCP only)

## Key Decisions

| Component | Choice | Rationale |
|-----------|--------|-----------|
| Destination resolution | Explicit page/database IDs | Most reliable; avoids broken search-by-title |
| API access | Notion MCP with IDs | Reuse existing MCP integration; only change resolution strategy |
| Todo skill relationship | Extend alongside existing | Keep working todo skill; add Notion variant as option |
| Configuration | Environment variables | IDs stored in `.env`, never committed to repo |

## Constraints

- Must use Notion MCP (no direct API calls)
- No fallback to search-by-title under any circumstances
- All operations must succeed with only explicit IDs
- Must not break existing todo skill functionality
- Never log or echo raw Notion IDs — print only named references
- Build and lint must pass
