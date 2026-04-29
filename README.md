# agentic-setup

A project template for AI-assisted TypeScript development. Provides a complete, opinionated setup for working with AI coding tools — structured enough to produce consistent results, flexible enough to fit any project.

Works with **Claude Code**, **Codex**, and any tool that reads instruction files or supports skills.

---

## What's included

| What | Where | Purpose |
|------|-------|---------|
| Skills | `.agents/skills/` | Tool-agnostic skill files (Codex + others) |
| Skills (symlinked) | `.claude/skills/` | Same skills, auto-discovered by Claude Code |
| Claude settings | `.claude/settings.json` | Git mutation deny list, permissions |
| MCP servers | `.mcp.json` | Context7 + Playwright pre-wired |
| Instruction file | `CLAUDE.md` | Claude Code project context |
| Instruction file | `AGENTS.md` | Codex project context (kept in sync) |
| Setup guide | `SETUP.md` | Per-machine tool installation |
| Project spec | `docs/spec/` | Product brief, tech spec, data model |
| Milestone tracker | `docs/work/now.md` | Active milestone and next action |
| Milestone history | `docs/work/milestones/` | Per-milestone specs, plans, checkpoints |
| Conventions | `docs/agents/conventions.md` | Project coding conventions |
| Commands | `docs/agents/commands.md` | Dev, test, build, lint commands |
| Tool registry | `docs/agents/recommended-tools.md` | All available skills, MCPs, and plugins |
| LLM docs | `docs/references/llms.md` | `llms.txt` / `llms-full.txt` URLs for used libraries |

---

## Quick start

### 1. Clone or fork this template

```bash
git clone https://github.com/your-username/agentic-setup my-project
cd my-project
```

### 2. Run per-machine setup

Follow **SETUP.md** to install:
- Superpowers (Claude Code plugin or Codex plugin)
- TypeScript language server
- Any optional MCP servers your project needs

### 3. Add your project documentation

Populate `docs/spec/` with your project documentation:
- `product.md` — what you're building, features, V1 scope
- `tech.md` — tech stack decisions, architecture
- `data-model.md` — schema, relationships (if applicable)

See `docs/spec/README.md` for the full guide.

### 4. Initialize

Run the `project-init` skill to auto-fill `CLAUDE.md`, `AGENTS.md`, conventions, and commands from your spec:

**Claude Code:** type `/project-init` in the chat

**Codex:** ask `use the project-init skill`

Review what was filled in, correct any wrong inferences, then start building.

---

## Development flow

This template uses a **one-milestone-at-a-time** approach driven by superpowers skills. The flow per feature:

```
superpowers:brainstorming     → design the feature, write spec.md
grill-me                      → stress-test the design (optional but recommended)
superpowers:writing-plans     → write detailed implementation plan.md
superpowers:subagent-driven-development  or  superpowers:executing-plans
superpowers:verification-before-completion
superpowers:requesting-code-review
superpowers:finishing-a-development-branch
→ update docs/work/now.md → start next milestone
```

Save session state at any point with the `checkpoint` skill.

See `docs/work/milestones/README.md` for the milestone philosophy.

---

## Skills

Skills live in `.agents/skills/` (canonical) and are symlinked into `.claude/skills/` for Claude Code. Codex discovers them from `.agents/skills/` directly (if it supports project-level discovery) or globally via the plugin.

| Skill | When to use |
|-------|------------|
| `project-init` | Once, after adding docs to `docs/spec/` |
| `grill-me` | To stress-test a plan or design |
| `checkpoint` | To save session state mid-implementation |
| `frontend-design` | When building UI components or pages |

Superpowers skills are installed separately (see SETUP.md) and provide the core development workflow.

---

## AI tool compatibility

| Tool | How it's supported |
|------|--------------------|
| **Claude Code** | `CLAUDE.md` + `.claude/settings.json` + `.claude/skills/` (symlinked) + `.mcp.json` |
| **Codex** | `AGENTS.md` + `.agents/skills/` + `.mcp.json` + Superpowers plugin |
| **Cursor** | `CLAUDE.md` or `AGENTS.md` (Cursor reads both) + `.mcp.json` |
| **Others** | Point your tool at `CLAUDE.md` or `AGENTS.md` as the instruction file |

---

## Git rules

AI tools using this template are configured to **never run git mutations**. No `git add`, `commit`, `push`, `reset`, `checkout`, or any destructive git command. The AI suggests commands — you run them.

This is enforced via `.claude/settings.json` for Claude Code. For other tools, it's enforced via the instruction files.

---

## Customization

- **Add a skill:** create `<name>/SKILL.md` in `.agents/skills/`, then symlink into `.claude/skills/`
- **Add an MCP:** add the config to `.mcp.json` (see SETUP.md for snippets)
- **Project-specific rules:** add to the "Project-specific rules" section in `CLAUDE.md` and `AGENTS.md`
- **Update conventions:** edit `docs/agents/conventions.md` directly or re-run `project-init`

---

## Resources

- [Superpowers](https://github.com/obra/superpowers) — core workflow skills
- [Claude Code docs](https://code.claude.com/docs) — skills, MCP, settings
- `docs/agents/recommended-tools.md` — full tool ecosystem reference
