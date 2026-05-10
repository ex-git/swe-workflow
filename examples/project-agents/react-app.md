# Example AGENTS.md — React App

## Required Commands

| Check | Command |
|---|---|
| Typecheck | npm run typecheck |
| Lint | npm run lint |
| Tests | npm test |
| Build | npm run build |

## UI Rules

- Use existing design-system components before raw HTML.
- Keep data fetching in existing hooks.
- Do not add new state-management libraries.
- New async UI must include loading, empty, and error states.

## Testing Rules

- Prefer user-visible behavior tests.
- Do not snapshot large components unless existing convention requires it.
- Cover disabled, loading, empty, and error states where applicable.
