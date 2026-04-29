# Coding Conventions

> Populated by `project-init`. Update as the project evolves.

---

## Language & runtime

<!-- e.g.
- TypeScript strict mode (`"strict": true`)
- ESM only — no CommonJS
- Node 20+, no version shims
-->

## Naming

<!-- e.g.
- Files and folders: kebab-case
- React components: PascalCase
- Non-component functions/variables: camelCase
- Constants: UPPER_SNAKE_CASE
-->

## Types

<!-- e.g.
- `interface` for object shapes; no `I`/`T` prefix
- No `enum` — use `as const` + derived union type
- Avoid `any`; use `unknown` + narrowing
-->

## Imports

<!-- e.g.
- Use path aliases (`@/`, `~lib/`) — no deep relative paths
- No barrel files (index.ts re-exports) except at package boundaries
-->

## Testing

<!-- e.g.
- Test runner: Vitest
- Test files: co-located, `*.test.ts`
- E2E: Playwright, tests in `e2e/`
- Integration tests must use real DB — no mocking infrastructure
-->

## Framework-specific

<!-- Add sections per framework in your stack, e.g.:

### React
- Hooks only — no class components
- State: Zustand for global, useState/useReducer for local

### NestJS
- One module per feature domain
- Services contain business logic — controllers are thin

### Supabase
- RLS on every table — no bypass
- database.types.ts is generated output — never edit manually
-->
