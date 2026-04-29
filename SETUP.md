# Setup

One-time per-machine setup. Run these before starting development on any project using this template.

---

## 1. Superpowers

### Claude Code

```bash
claude plugin install superpowers@claude-plugins-official
```

### Codex CLI

```bash
/plugins
```

Search for `superpowers`, then select **Install Plugin**.

### Codex App

In the Codex app, click **Plugins** in the sidebar. Find **Superpowers** in the Coding section and click **+** next to it.

To enable subagent skills (`dispatching-parallel-agents`, `subagent-driven-development`), add to `~/.codex/config.toml`:

```toml
[features]
multi_agent = true
```

---

## 2. TypeScript Language Server (Claude Code LSP)

```bash
npm install -g typescript-language-server typescript
```

---

## 3. MCP Servers

### Context7 & Playwright

No setup needed — both run via `npx` on demand from `.mcp.json`.

For Playwright, install browser binaries once:

```bash
npx playwright install
```

### Optional MCPs

Add any of these to `.mcp.json` as needed for your project. See `docs/agents/recommended-tools.md` for the full list.

#### Supabase

```json
"supabase": {
  "url": "https://mcp.supabase.com/mcp",
  "headers": {
    "Authorization": "Bearer YOUR_SUPABASE_ACCESS_TOKEN"
  }
}
```

Get your access token: https://supabase.com/dashboard/account/tokens

#### Stripe

```json
"stripe": {
  "url": "https://mcp.stripe.com",
  "headers": {
    "Authorization": "Bearer YOUR_STRIPE_SECRET_KEY"
  }
}
```

#### Shadcn/ui

```json
"shadcn": {
  "command": "npx",
  "args": ["shadcn@latest", "mcp"]
}
```

#### Inngest (local dev server)

Start the Inngest dev server first (`npx inngest-cli@latest dev`), then add:

```json
"inngest-dev": {
  "url": "http://127.0.0.1:8288/mcp"
}
```

---

## 4. Initialize the project

After cloning this template and adding your docs to `docs/spec/`, run the init skill:

**Claude Code:** `/project-init`

**Codex:** `use the project-init skill`
