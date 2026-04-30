# Recommended Tools, Skills & MCPs

A living reference of the tools ecosystem for this template. Add or remove per project.

---

## Local skills (`.agents/skills/`)

Bundled in the template:

| Skill | Trigger | What it does |
|-------|---------|-------------|
| `checkpoint` | "save context" / "/checkpoint" | Saves a resumable session snapshot to the active milestone folder |

`/project-init` is intentionally **not** registered as a standard skill — skills load into agent context every session, but project-init runs once per project. Its body lives at `docs/agents/project-init.md`, exposed via `.claude/commands/project-init.md` for Claude Code and as a natural-language fallback for other tools.

---

## Superpowers (installed separately — see SETUP.md)

The core workflow engine. Per-milestone chain:

| Skill | When |
|-------|------|
| `superpowers:brainstorming` | Design exploration → spec |
| `superpowers:writing-plans` | Detailed step-by-step implementation plan |
| `superpowers:subagent-driven-development` | Implement (preferred for non-trivial milestones) |
| `superpowers:executing-plans` | Implement (simpler alternative) |
| `superpowers:verification-before-completion` | Verify before claiming done |
| `superpowers:requesting-code-review` | Pre-merge review |
| `superpowers:receiving-code-review` | Process review feedback rigorously |
| `superpowers:finishing-a-development-branch` | Wrap up the milestone |

Cross-cutting:

| Skill | When |
|-------|------|
| `superpowers:systematic-debugging` | Structured debugging before proposing a fix |
| `superpowers:test-driven-development` | TDD red-green-refactor loop |
| `superpowers:dispatching-parallel-agents` | Parallelize independent tasks |

---

## Matt Pocock skills (installed separately — see SETUP.md)

Curated subset that complements Superpowers:

| Skill | When |
|-------|------|
| `grill-with-docs` | Stress-test a plan against the project's domain language; updates `CONTEXT.md` and ADRs inline. Use instead of plain `grill-me`. |
| `caveman` | Ultra-compressed response mode (~75% token reduction). |
| `improve-codebase-architecture` | Surface deepening opportunities; run periodically. |
| `zoom-out` | Get a higher-level perspective on an unfamiliar code area. |
| `write-a-skill` | Author your own skills with proper structure. |

Skip the rest:

- `tdd`, `diagnose` — covered by Superpowers (`test-driven-development`, `systematic-debugging`).
- `to-prd`, `to-issues`, `triage`, `setup-matt-pocock-skills` — GitHub / Linear issue-tracker workflows that don't fit milestone folders.
- `git-guardrails-claude-code` — `.claude/settings.json` deny list already covers it.
- `migrate-to-shoehorn`, `scaffold-exercises`, `setup-pre-commit` — niche / stack-specific.

---

## Optional skills (Claude Code plugins)

Install with `claude plugin install <name>@claude-plugins-official`:

| Plugin | When to add |
|--------|------------|
| `ai-elements` | AI chat UI components (React) |
| `supabase` | Supabase MCP + skills |
| `sentry` | Error monitoring |
| `firecrawl` | Web scraping / research |
| `chrome-devtools-mcp` | Performance debugging |
| `typescript-lsp` | TS code intelligence |
| `playwright` | E2E testing |

Do **not** install:

- `commit-commands` — agents must not run git mutations
- `feature-dev` — Superpowers replaces it

---

## MCP servers

Pre-wired in `.mcp.json` (no setup beyond install):

| MCP | What it does |
|-----|-------------|
| `context7` | Pulls library docs on demand. Optional override per library in `docs/references/llms.md`. |
| `playwright` | Browser automation for E2E testing |

Optional — add to `.mcp.json` under `mcpServers` (see SETUP.md for snippets):

| MCP | URL / command | Auth | Best for |
|-----|---------------|-----|---------|
| `supabase` | `https://mcp.supabase.com/mcp` | access token | Supabase DB, auth, storage |
| `stripe` | `https://mcp.stripe.com` | secret key | Stripe payments |
| `shadcn` | `npx shadcn@latest mcp` | none | Shadcn/ui generation |
| `inngest-dev` | `http://127.0.0.1:8288/mcp` | local | Inngest dev server |
| `sentry` | via plugin | auth token | Error monitoring |
| `firecrawl` | via plugin | API key | Web scraping |
| `chrome-devtools` | via plugin | none | Performance debugging |
