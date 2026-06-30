# Session — New Agentic Workflow Plugin

_Created: 2026-06-30. Status: Pre-planning. Compaction pending, then fresh brainstorm._

---

## What we're doing

Building a **new version of the agentic-setup workflow** — a curated plugin that combines and modifies skills from multiple sources. This is NOT another clean-framework-from-scratch effort. It's a practical, shippable combination of existing skill libraries, modified where needed.

**Why:** Building the custom framework from scratch is taking too long. This approach gets something working fast. Custom framework work continues separately after this unblocks the user.

---

## Sources being gathered

| Source | Location | Status |
|---|---|---|
| Superpowers | `temp/repos/superpowers/` | Already cloned. Skills analyzed. |
| agent-toolkit | `temp/repos/agent-toolkit/` | Already cloned. Catalogued at `temp/refs/agent-toolkit.md`. |
| mattpocock skills | TBD | Not yet cloned. Need URL/repo name. |
| taste-skill | `temp/refs/taste-skill.md` (reference notes) | Not yet tried. Evaluate on a real UI task first. |
| ui-ux-pro-max-skill | TBD | Not yet seen or cloned. |

---

## Open questions (answer these before brainstorming)

1. **mattpocock skills** — What is the repo URL? Is this TypeScript/Zod-related, or frontend/UI skills?

2. **ui-ux-pro-max-skill** — What is this? Repo name or link?

3. **Plugin location** — Where does the new plugin live? Options:
   - New standalone repo (e.g., `~/code/projects/my-plugin/`)
   - A folder inside `agentic-setup/`
   - Somewhere else?

4. **Installation target** — Does this replace Superpowers entirely (user removes Superpowers, installs new plugin), or sit alongside it?

5. **Any other skill sources** to gather before we start planning?

---

## Known problems to solve in the new plugin

### From Superpowers (must fix)

- **Brainstorming HARD-GATE**: Forces a 9-step mandatory process (context explore → questions → 2-3 approaches → design → write design doc → commit → self-review → user review → transition to writing-plans). Every step mandatory even for tiny tasks. → Needs to become lightweight, conversational, no forced doc writing, no commit step.
- **using-superpowers "1% chance" rule**: Causes constant over-invocation of skills. → Soften to "if it clearly applies."
- **Subagent-driven-development**: Was prohibited on Delapse; Superpowers kept recommending it. → Neuter or remove, OR replace with a smarter version.
- **Git mutations in skill steps**: Brainstorming step 6 says "commit". Must be removed everywhere.
- **AskUserQuestion tool**: Must be prohibited globally.
- **Post-compaction reset**: Skills re-applied as if overrides didn't exist after compaction. → Fix in CLAUDE.md so hard rules survive.
- **Context explosion**: ~90-100K tokens consumed at session start. → Minimize what loads upfront.

### New skills needed (not in Superpowers)

- **db-design guide**: Brainstorm complete (see `temp/refs/db-design-skill/db-design-guide-notes.md`). Base: agent-toolkit Skill-4, modified. Ready to write any session.
- **debug guide**: Core principle — agent must not state a cause without proof. "Hypothesis: X might cause this → verify by doing Y." Must replace the assumption-heavy debugging behavior.
- **backend-design guide** (name TBD): Domain/service organization, route hierarchy, API naming. Covers Delapse Issues 4, 6, 7. Needs its own brainstorm session.
- **brainstorm guide**: Draft exists at `framework-build/docs/guides/core/brainstorm.md` — treat as scratch only. Needs full redesign session.
- **execute/handoff/verify guides**: Still pending.

---

## Superpowers skills inventory

| Skill | Keep / Modify / Remove |
|---|---|
| `brainstorming` | Modify heavily — strip HARD-GATE and forced steps |
| `writing-plans` | Keep or modify — needs separate brainstorm to assess |
| `executing-plans` | Keep or modify — needs separate brainstorm to assess |
| `systematic-debugging` | Modify — add proof-required rule |
| `verification-before-completion` | Likely keep — assess |
| `requesting-code-review` | Likely keep — assess |
| `receiving-code-review` | Likely keep — assess |
| `finishing-a-development-branch` | Assess — may be too opinionated |
| `subagent-driven-development` | Remove or replace with smarter version |
| `test-driven-development` | Remove (D3: no TDD) |
| `using-git-worktrees` | Remove (not wanted) |
| `dispatching-parallel-agents` | Assess |
| `using-superpowers` | Modify — soften invocation rule, rename |
| `writing-skills` | Keep for reference |

---

## agent-toolkit skills to include

High priority (from `temp/refs/agent-toolkit.md`):
- `session-handoff` — overlaps with our handoff.md need
- `gepetto` — multi-step planning reference
- `game-changing-features` — strategic brainstorming
- `requirements-clarity` — requirements gathering
- `reducing-entropy` — code quality

DB + stack:
- `database-schema-designer` — already analyzed, basis for db-design guide

Visualization (evaluate):
- `excalidraw`, `draw-io`, `c4-architecture`, `mermaid-diagrams`

---

## taste-skill / ui-ux-pro-max-skill

- **taste-skill**: Anti-slop frontend skill. GSAP dependency may conflict with shadcn/Tailwind stack. Try on a real UI task before committing. See `temp/refs/taste-skill.md`.
- **ui-ux-pro-max-skill**: Unknown — need to see it first.

---

## On resume

1. Read this file
2. Answer the open questions above (user provides: mattpocock URL, ui-ux-pro-max info, plugin location decision)
3. Gather remaining sources (clone mattpocock, look at ui-ux-pro-max)
4. Then brainstorm: what goes in, what gets modified, what the plugin structure looks like
5. Do NOT start making file changes until the plan is clear

_Updated: 2026-06-30._
