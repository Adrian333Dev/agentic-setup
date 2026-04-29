# Recommended Tools, Skills & MCPs

A living reference of the tools ecosystem for this template. Add or remove per project.

---

## Skills (`.agents/skills/`)

Skills included in this template:

| Skill | Trigger | What it does |
|-------|---------|-------------|
| `project-init` | Once on setup | Reads `docs/spec/` and populates all config files |
| `grill-me` | "grill me" / stress-test a design | Adversarial Q&A to surface blind spots |
| `checkpoint` | "save context" / end of session | Saves resumable session snapshot |
| `frontend-design` | Building UI components or pages | High-quality, non-generic frontend code |

Skills installed separately (see SETUP.md):

| Skill | Source | What it does |
|-------|--------|-------------|
| `superpowers:brainstorming` | `obra/superpowers` | Design exploration → spec |
| `superpowers:writing-plans` | `obra/superpowers` | Detailed step-by-step implementation plan |
| `superpowers:executing-plans` | `obra/superpowers` | Execute a plan inline with checkpoints |
| `superpowers:subagent-driven-development` | `obra/superpowers` | Dispatch fresh subagent per task |
| `superpowers:systematic-debugging` | `obra/superpowers` | Structured debugging before proposing fixes |
| `superpowers:test-driven-development` | `obra/superpowers` | TDD workflow |
| `superpowers:requesting-code-review` | `obra/superpowers` | Pre-merge review |
| `superpowers:receiving-code-review` | `obra/superpowers` | Process review feedback rigorously |
| `superpowers:verification-before-completion` | `obra/superpowers` | Verify before claiming done |
| `superpowers:finishing-a-development-branch` | `obra/superpowers` | Wrap up a milestone |
| `superpowers:dispatching-parallel-agents` | `obra/superpowers` | Parallelise independent tasks |

Optional skills to add as needed:

| Skill | Source | What it does |
|-------|--------|-------------|
| `ai-elements` | `claude-plugins-official` | AI chat UI components (React) |
| `supabase:supabase` | `supabase@claude-plugins-official` | Supabase-aware workflows |
| `sentry:sentry-workflow` | `sentry@claude-plugins-official` | Fix issues from Sentry context |
| `firecrawl:firecrawl` | `firecrawl@claude-plugins-official` | Web scraping and research |

---

## MCP Servers

Pre-wired in `.mcp.json` (no setup beyond install):

| MCP | What it does |
|-----|-------------|
| `context7` | Pulls up-to-date library docs on demand — prefer `llms.md` entries when available |
| `playwright` | Browser automation for E2E testing |

Optional — add to `.mcp.json` as needed (see SETUP.md for config snippets):

| MCP | URL / command | Needs auth | Best for |
|-----|---------------|-----------|---------|
| `supabase` | `https://mcp.supabase.com/mcp` | Yes — access token | Supabase DB, auth, storage |
| `stripe` | `https://mcp.stripe.com` | Yes — secret key | Stripe payments |
| `shadcn` | `npx shadcn@latest mcp` | No | Shadcn/ui component generation |
| `inngest-dev` | `http://127.0.0.1:8288/mcp` | No (local) | Inngest local dev server |
| `sentry` | via plugin | Yes — auth token | Error monitoring |
| `firecrawl` | via plugin | Yes — API key | Web scraping |
| `chrome-devtools` | via plugin | No | Performance debugging |

---

## Claude Code Plugins

Install with `claude plugin install <name>@claude-plugins-official`:

| Plugin | What it adds | When to add |
|--------|-------------|------------|
| `superpowers` | Core workflow skills (brainstorming, planning, TDD, debugging, review) | Always — see SETUP.md |
| `frontend-design` | UI design skill | Web projects |
| `supabase` | Supabase MCP + skills | Supabase projects |
| `sentry` | Sentry MCP + skills | Projects with error monitoring |
| `firecrawl` | Firecrawl MCP + skill | Research-heavy workflows |
| `chrome-devtools-mcp` | Chrome DevTools MCP | Performance debugging |
| `typescript-lsp` | TypeScript LSP server | All TS projects (improves code intelligence) |
| `playwright` | Playwright MCP | E2E testing |

Do **not** install:
- `commit-commands` — AI must not run git mutations
- `feature-dev` — superpowers replaces it
