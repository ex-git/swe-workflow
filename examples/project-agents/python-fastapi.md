# Example AGENTS.md — Python FastAPI

## Required Commands

| Check | Command |
|---|---|
| Typecheck | pyright |
| Lint | ruff check . |
| Format check | ruff format --check . |
| Tests | pytest |

## API Rules

- Use Pydantic models for request and response validation.
- Preserve existing exception handling patterns.
- Keep DB access in service or repository layer.
- Add tests for 200, 400, 401/403, and relevant failure behavior.

## Data Rules

- Every schema change requires a migration.
- Destructive migrations require rollback notes.
- Backfills must be idempotent.
