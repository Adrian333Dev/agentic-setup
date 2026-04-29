---
name: project-init
description: Initialize this project from docs/spec. Run once after cloning the template and populating docs/spec/ with your project documentation. Reads all spec files and fills in CLAUDE.md, AGENTS.md, docs/agents/conventions.md, docs/agents/commands.md, and docs/work/now.md.
---

# Project Initialization

Run this skill once when starting a new project from the agentic-setup template.

## Prerequisites check

Before proceeding, check whether `docs/spec/` contains actual project documentation beyond the README placeholder. If it only has the README, stop immediately and tell the user:

> "Please add your project documentation to `docs/spec/` first (product brief, tech spec, data model, etc.), then run this skill again."

---

## Step 1 — Read all spec docs

Read every file under `docs/spec/`. Extract and note:
- Project name, one-sentence description
- Project type (web-app / web-extension / desktop / mobile / library / other)
- Full tech stack: **every** framework, library, and service mentioned — be specific (e.g. "React 19 + Vite 6 + TanStack Router" not just "React")
- Monorepo structure if applicable
- V1 scope: what is and isn't in the first version
- Any team conventions or preferences mentioned explicitly

---

## Step 2 — Fill CLAUDE.md and AGENTS.md

Replace every `<!-- ... -->` placeholder in both files with real values from the spec. Keep both files in sync — identical content, different filenames.

---

## Step 3 — Fill docs/agents/commands.md

Scan the project for `package.json` (root + workspaces), `turbo.json`, `nx.json`, `Makefile`, and any `scripts/` directory. Document every relevant command: dev server, test, build, type-check, lint, format, DB migrations, code generation.

If the project isn't fully initialized yet, note which commands couldn't be verified and mark them with `# unverified`.

---

## Step 4 — Research and fill docs/agents/conventions.md

This step requires genuine research — do not fill this file with generic placeholders or vague rules.

### 4a. Identify the stack precisely

List every major framework and library that will shape coding conventions. Examples: TypeScript, React, NestJS, Prisma, Drizzle, Supabase, Zustand, TanStack Query, Zod, Vitest, Playwright.

### 4b. Research each framework's conventions

For each major framework/library, use **context7** (or the `llms.txt` entries in `docs/references/llms.md` if available) to look up:
- Official style guide / best practices
- Recommended file structure
- Framework-specific anti-patterns to avoid
- Common conventions for the specific version in use

Prioritize: frameworks that impose the most structure (NestJS, Next.js, Nuxt) and testing tools (Vitest, Playwright).

### 4c. Ask the user about preferences that can't be inferred

Ask these questions **one at a time** before filling in the file:

1. What test runner are you using? (Vitest / Jest / other)
2. For frontend state: local-first with `useState` / global store (Zustand, Jotai, Redux)?
3. Import paths: path aliases (e.g. `@/`, `~lib/`) or relative? Any barrel file policy?
4. Any naming conventions that differ from standard TS? (e.g. file naming, component suffixes)
5. Anything else you want explicitly enforced that wouldn't be obvious from the stack?

### 4d. Write specific, actionable rules

Every line in `docs/agents/conventions.md` must be a **concrete, followable rule** that removes ambiguity. Examples of what to write vs. what not to write:

| Good | Bad |
|------|-----|
| `Use \`interface\` for object shapes; never \`type\` for objects` | `Follow TypeScript best practices` |
| `No \`enum\` — use \`as const\` + derived union: \`type Status = typeof STATUS[keyof typeof STATUS]\`` | `Avoid enums` |
| `Test files: co-located alongside source, named \`*.test.ts\`` | `Write tests` |
| `NestJS modules: one per feature domain in \`src/modules/<domain>/\`` | `Follow NestJS conventions` |

Fill every section with real rules. If a section genuinely doesn't apply (e.g. no database = no DB conventions), remove that section entirely rather than leaving placeholders.

---

## Step 5 — Initialize docs/work/now.md

Set the first milestone:
- Derive it from the V1 scope in the spec — what's the natural starting point?
- If the spec has a clear first step (e.g. "auth before anything else"), use it
- If genuinely ambiguous, propose a milestone and ask the user to confirm before writing

---

## Step 6 — Report

Output a summary:
- What was filled in automatically
- What was filled in via research (and which sources were used)
- What required user input (and what was decided)
- What still needs manual review — call these out explicitly

Then tell the user:

> "Initialization complete. Please review:
> - `CLAUDE.md` and `AGENTS.md` — verify the inferred context and rules
> - `docs/agents/conventions.md` — the most important file to get right; correct anything that doesn't match your team's approach
> - `docs/agents/commands.md` — verify commands, especially any marked `# unverified`
> - `docs/work/now.md` — confirm the first milestone
>
> When ready, start the first milestone with `superpowers:brainstorming`."
