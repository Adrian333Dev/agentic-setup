# <!-- Project Name -->

<!-- One-sentence project description -->

> This file is the Codex equivalent of CLAUDE.md. Keep them in sync.
> Superpowers for Codex is installed globally — see SETUP.md.

---

## Project context

> Fill in after running the `project-init` skill.

- **Name:** <!-- project name -->
- **Type:** <!-- web-app | web-extension | desktop | mobile | library | other -->
- **Tech stack:** <!-- e.g. TypeScript, React, Vite, NestJS, Supabase, pnpm -->
- **Monorepo:** <!-- yes (Turborepo / pnpm workspaces) | no -->
- **Repo structure:** <!-- brief description, e.g. apps/web + apps/api + packages/contracts -->

## Project-specific rules

> Add rules that emerge from your spec and can't be inferred from conventions.

<!-- e.g.
- Never write raw SQL — use the query builder
- All app data goes through apps/api — browser never reads DB directly
-->

---

## Session start

1. Read `docs/work/now.md`
2. If the active milestone folder has `session.md`, read it and stop — don't read source files unless the current task requires them

## Hard rules

- **Never run git mutations** — no `git add`, `commit`, `push`, `reset`, `checkout`, `rebase`, `merge`, `stash`, `clean`, or any branch/worktree mutations. Suggest the command for the user to run.
- **Never pre-define a list of future milestones** — define one at a time. The next milestone is planned only after the current one is complete.
- **Milestone scope** — one feature per milestone. Split into sub-milestones if scope grows unexpectedly.
- **No placeholders in plans** — every task in a plan must contain real file paths, real code, real commands.

## Superpowers path overrides

- Specs → `docs/work/milestones/<slug>/spec.md`
- Plans → `docs/work/milestones/<slug>/plan.md`

## Key docs

| Doc | Purpose |
|-----|---------|
| `docs/work/now.md` | Active milestone and next action |
| `docs/spec/` | Project bible — product, tech, data model, decisions |
| `docs/agents/conventions.md` | Coding conventions for this project |
| `docs/agents/commands.md` | Dev, test, build, and lint commands |
| `docs/references/llms.md` | LLM-ready doc URLs for tools used here |
| `docs/agents/recommended-tools.md` | Skills, MCPs, and optional tools |
