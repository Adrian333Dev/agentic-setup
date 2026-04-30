# <!-- Project Name -->

<!-- One-sentence project description -->

> Designed for **solo developers**. Workflow assumes one author, one branch context, no team coordination layer (no PR templates, code-owners, multi-reviewer gates).

---

## Project context

> Fill in after running `/project-init`.

- **Name:** <!-- project name -->
- **Stack:** <!-- e.g. TypeScript, React, Vite, NestJS, Supabase, pnpm -->
- **Structure:** <!-- e.g. single app | monorepo (apps/web + apps/api + packages/contracts) | extension + dashboard | library -->

## Project-specific rules

> Add rules that emerge from your spec and can't be inferred from conventions.

<!-- e.g.
- Never write raw SQL — use the query builder
- All app data goes through apps/api — browser never reads DB directly
-->

---

## Session start

1. Read `docs/work/now.md` to find the active milestone and next action.
2. If the active milestone folder has `session.md`, read it before reading source files.
3. The next action in `now.md` is usually a **Superpowers** skill — invoke it rather than improvising.

## Workflow — Superpowers as the engine

This template's design → plan → implement → verify → ship loop runs on the Superpowers skill suite. Per milestone:

```
superpowers:brainstorming                    → spec.md
[grill-me]                                   → stress-test the spec
superpowers:writing-plans                    → plan.md
superpowers:subagent-driven-development      → implement (or executing-plans for simpler work)
[checkpoint mid-session]                     → session.md
superpowers:verification-before-completion   → verify against plan + conventions
superpowers:requesting-code-review           → pre-merge review
superpowers:finishing-a-development-branch   → wrap, update now.md, adjust roadmap.md
```

Skills override default agent behavior. When `now.md` points to one, use it.

`grill-with-docs` is an opt-in alternative to `grill-me`, useful once the project has accumulated domain terminology. It writes to repo-root `CONTEXT.md` and `docs/adr/` (not the milestone folder) — invoke it deliberately, not by default.

## Hard rules

- **Never run git mutations** — no `git add`, `commit`, `push`, `reset`, `checkout`, `rebase`, `merge`, `stash`, `clean`, or any branch/worktree mutations. Suggest the command for the user to run.
- **One formal milestone at a time** — only one milestone has a `spec.md` + `plan.md` at any moment. The next milestone is formalized only after the current one ships. A loose `docs/work/roadmap.md` of upcoming work is fine and expected; it's not a commitment.
- **Milestone scope** — one feature per milestone. Split into sub-milestones (`m01a-…`, `m01b-…`) if scope grows.
- **No placeholders in plans** — every task in `plan.md` must contain real file paths, real code, real commands.
- **Maintain `docs/agents/conventions.md` and `docs/agents/commands.md` as living documents** — when you discover or apply a convention or command not listed there, suggest adding it.

## Superpowers path overrides

- Specs → `docs/work/milestones/<slug>/spec.md`
- Plans → `docs/work/milestones/<slug>/plan.md`

## Key docs

| Doc | Purpose |
|-----|---------|
| `docs/work/now.md` | Active milestone and next action |
| `docs/work/roadmap.md` | Loose blueprint of upcoming work — not a commitment |
| `docs/spec/` | Project bible (product + tech) |
| `docs/agents/conventions.md` | Coding conventions — base + stack layer; living document |
| `docs/agents/commands.md` | Dev/test/build/lint commands; living document |
| `docs/agents/recommended-tools.md` | Skills, MCPs, plugins reference |
| `docs/references/llms.md` | Optional `llms.txt` overrides for specific libraries |
