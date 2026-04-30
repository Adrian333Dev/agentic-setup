# Coding Conventions

This file is a **living document**. The template ships with a base layer of universal + TS/JS conventions. `/project-init` enhances it with stack-specific rules. As you encounter or apply new conventions during milestones, add them here.

Every rule must be **concrete and followable**. Vague guidelines like *"follow best practices"* do not belong here.

---

## Universal

### Naming

- Files and folders: `kebab-case` (`user-profile.tsx`, `auth-provider/`).
- Components: `PascalCase` (`UserProfile`).
- Variables and functions: `camelCase` (`getUserProfile`).
- Constants and enum-like consts: `UPPER_SNAKE_CASE` (`MAX_RETRY_COUNT`).
- Types and interfaces: `PascalCase` — no `I` or `T` prefix (`User`, not `IUser`; `Result`, not `TResult`).
- Boolean variables: prefix with `is`, `has`, `can`, `should` (`isLoading`, `hasAccess`).
- Event handlers: `on*` for the prop, `handle*` for the implementation (`<button onClick={handleSubmit}>`).

### Strings and constants

- No hardcoded user-facing strings inline — centralize in a `constants/` module or i18n layer with typed keys.
- No magic numbers — name them as `UPPER_SNAKE_CASE` constants near use, or in a shared `constants` module.
- No magic enum values — use `as const` lookup objects with derived union types.

### Imports

- Prefer path aliases (`@/`, `~/`) over deep relative paths (`../../../`).
- No barrel files (`index.ts` re-exports) except at package boundaries — they hurt tree-shaking and create circular import hazards.
- Group imports: external packages → internal aliases → relative → types.

### Functions

- Small, single-purpose. If a function does two things, split it.
- Pure where possible — no side effects unless the function name signals it (`save…`, `fetch…`, `track…`).
- Prefer early returns over deep nesting.
- Don't add error handling, fallbacks, or validation for scenarios that can't happen. Trust internal code; validate at system boundaries (user input, external APIs).

### Errors

- Never throw raw strings — throw `Error` instances or domain error classes.
- Never swallow errors silently. Log, rethrow, or convert to a typed result.
- Use `unknown` in catch blocks and narrow before use.

### Comments

- Default: write no comments. Well-named identifiers are the documentation.
- Add a comment only when WHY is non-obvious: a hidden constraint, a subtle invariant, a workaround for a specific bug, behavior that would surprise a reader.
- Don't explain WHAT the code does. Don't reference current task / fix / callers — that belongs in the commit message.
- No multi-paragraph docstrings.

### Abstraction

- No premature abstraction. Three similar lines is better than a wrong helper.
- Don't design for hypothetical future requirements.
- Don't add half-finished implementations or "we might need this later" hooks.

### Tests

- Co-located with source: `user-profile.tsx` + `user-profile.test.tsx`.
- Test names describe behavior, not implementation: *"redirects unauthenticated users to login"*, not *"calls router.push"*.
- Test public interfaces, not internals. If renaming an internal function breaks a test, the test was wrong.

---

## TypeScript / JavaScript

> Replace this section with the appropriate language conventions if your project isn't TS/JS-based.

### Types

- `interface` for object shapes; `type` for unions, intersections, and mapped types.
- No `enum` — use `as const` + a derived union:
  ```ts
  const STATUS = { idle: 'idle', loading: 'loading', error: 'error' } as const;
  type Status = (typeof STATUS)[keyof typeof STATUS];
  ```
- Avoid `any` — use `unknown` and narrow with type guards. `any` is allowed only as a documented last resort with a comment explaining why.
- Prefer `readonly` for arrays and objects passed across module boundaries.
- Prefer template literal types and `satisfies` for compile-time validation over runtime checks where possible.

### Async

- `async/await` over raw `.then()` chains.
- Always handle rejections — never leave a floating promise.
- For parallel work, choose `Promise.all` (fail-fast) or `Promise.allSettled` (collect-all) deliberately, not by reflex.

### Modules

- ESM only — no CommonJS unless a dependency forces it.
- Named exports preferred over default exports — better refactoring, no naming drift.

### Null vs undefined

- Pick one for "no value" and stick to it across the codebase. Default: `undefined` for "not set yet", `null` only for "explicitly nothing" (e.g. database NULL).

---

## Stack-specific

> Populated by `/project-init` based on the libraries in your spec. Add new sections as you adopt new libraries.

<!-- Examples (will be filled by /project-init):

### TanStack Query
- Cache keys via stable factory: `queryKeys.user.byId(id)` — never inline arrays.
- One custom hook per query (`useUser`) and per mutation (`useUpdateUser`).
- Invalidate by key prefix, not refetch.

### NestJS
- One module per feature domain in `src/modules/<domain>/`.
- Services hold business logic; controllers stay thin.
- DTOs validated with class-validator at the boundary.

-->
