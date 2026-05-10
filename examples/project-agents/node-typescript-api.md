# Example AGENTS.md — Node TypeScript API

## Required Commands

| Check | Command |
|---|---|
| Typecheck | npm run typecheck |
| Lint | npm run lint |
| Unit tests | npm test |
| Build | npm run build |

## Architecture Rules

- Route handlers must stay thin.
- Business logic belongs in services.
- Database access belongs in repositories.
- Shared request/response types live in `src/types`.

## API Rules

- Validate request bodies with the existing schema validator.
- Preserve existing error shape: `{ error: { code, message } }`.
- Every new route needs auth middleware unless explicitly public.
- Update OpenAPI docs when request or response shape changes.

## Data Rules

- Do not query the database directly from route handlers.
- New list endpoints must support pagination.
- New query paths must consider indexes.

## Testing Rules

- Add route tests for 200, 400, and 401/403 where applicable.
- Add service tests for core business logic.
