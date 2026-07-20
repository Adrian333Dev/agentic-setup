# Session — New Agentic Workflow Plugin

_Last updated: 2026-07-19 (session 11). **Repo reorg done** — this dir is now a **container** holding `agentic-workflow_v2/` (the template, renamed from `agentic-setup_v2`, own git repo) + `skills/` (new skills catalog, own git repo); design docs stay in `new-workflow/`; v1 + `framework-build` + external clones archived to `reference/`. See the **"Latest"** section directly below. TWO design threads: (1) **`project-init` / front-of-lifecycle** — `new-workflow/design-project-genesis.md` — essentially CLOSED (branches #1–#5, #8 resolved; #6 topology parked). (2) **Skill & knowledge ecosystem (#7)** — `new-workflow/design-skill-ecosystem.md` — **branches #1–#4 CLOSED, next = branch #5 (init-integration seam).** `agentic-workflow_v2/` is the primary artifact; 7 skills built + `maintaining-skills` confirmed (not yet built)._

**Scope note:** this file tracks work under `new-workflow/` only. Do NOT edit `framework-build/` files without explicit instruction. **Relationship clarified 2026-07-01 (session 2):** `new-workflow/` is effectively framework-build v2 — same goal (replace the rigid Superpowers workflow), different method (curate/adapt existing Claude/Codex-compatible skill ecosystems instead of building a from-scratch guide system). `framework-build/` may be fully rewritten or abandoned — not certain. Its locked decisions (`framework-build/docs/design-session.md`, 85 decisions) are now treated as a **reference/source of validated thinking** to pull into new-workflow — read freely, cite freely, port decisions over. Just don't *write* to framework-build/ files without being told to.

---

## Latest — 2026-07-19 (session 11): repo reorg + skills catalog scaffolded

**This `agentic-setup/` dir is now the CONTAINER**, not a workflow itself. Structure locked:

- `agentic-workflow_v2/` — the template (Repo A). **Renamed from `agentic-setup_v2/`** and `git init`'d (own `.git`) by the user this session.
- `skills/` — the personal skills catalog (Repo B), **newly scaffolded + `git init`'d** (own `.git`). Mirrors mattpocock's install-critical shape: `.claude-plugin/plugin.json` (name `adrian-skills`, empty `skills[]`), flat `skills/`, `README.md`, `CLAUDE.md` authoring guide, `.gitignore`. Installs via `npx skills add Adrian333Dev/skills` once pushed + a first `SKILL.md` exists. **Buckets deferred** (flat start, no `misc`); **versioning/changesets deferred** (branch #4). CLI finding: `npx skills init` is **per-skill only** (`<name>/SKILL.md`) — there is no repo-scaffold command; `add` works on any repo whose `SKILL.md` files carry `name`+`description` frontmatter.
- `new-workflow/` — design lab (design docs + `research-log/`), now clone-free.
- `reference/` — read-only: `v1-template/` (archived old root `README/SETUP/AGENTS/CLAUDE` + `docs/`), `framework-build/`, and the 7 external clones moved out of `new-workflow/` (mattpocock's → `reference/mattpocock-skills/`).
- New root `CLAUDE.md` = **container/workbench instructions** (NOT the template's shipped CLAUDE.md, which stays deferred until v2's skills are complete). Kept in place: `.claude/settings.json` + `settings.local.json`, `.codex/config.toml`, `.mcp.json` (context7 + playwright).

**Pending git (user runs — never the agent):** `git rm -r --cached agentic-workflow_v2 skills` so the parent stops tracking the two nested repos; gitignore the external clones under `reference/`; push `skills/` to `Adrian333Dev/skills`.

**Still open:** archive orphaned v1 `.claude/commands/*` + `.agents/skills/checkpoint/` into `reference/v1-template/` (flagged, awaiting go). **NEXT design action unchanged: branch #5 (init-integration seam).** First catalog skill still to be authored (via `npx skills init <name>`).

---

## What we're doing

Building a **new version of the agentic-setup workflow** — a curated plugin that combines and modifies skills from multiple sources. This is NOT another clean-framework-from-scratch effort. It's a practical, shippable combination of existing skill libraries, modified where needed.

**Why:** Building the custom framework from scratch is taking too long. This approach gets something working fast. Custom framework work continues separately after this unblocks the user.

---

## Sources being gathered

All cloned; **relocated to `reference/` on 2026-07-19** (superseding the `new-workflow/` paths below, and earlier `temp/repos/`). mattpocock's clone was renamed to `reference/mattpocock-skills/` so it doesn't clash with the new `skills/` catalog repo:

| Source | Origin repo | Location | Status |
|---|---|---|---|
| Superpowers (fork) | `Adrian333Dev/superpowers` | `reference/superpowers/` | Cloned, currently identical to upstream `obra/superpowers` v6.1.0. Being actively modified — see Pivot execution below. |
| agent-toolkit | `softaworks/agent-toolkit` | `reference/agent-toolkit/` | Cloned. Catalogued at `temp/refs/agent-toolkit.md`. Not yet curated. |
| mattpocock skills | `mattpocock/skills` | `reference/mattpocock-skills/` | Cloned. Categories: deprecated, engineering, in-progress, misc, personal, productivity. Not yet catalogued in detail. |
| taste-skill | `Leonxlnx/taste-skill` | `reference/taste-skill/` | Cloned. Reference notes at `temp/refs/taste-skill.md`. Not yet tried on a real task. |
| ui-ux-pro-max-skill | `nextlevelbuilder/ui-ux-pro-max-skill` | `reference/ui-ux-pro-max-skill/` | Cloned. "161 reasoning rules, 67 UI styles" per its README. Not yet evaluated in depth. |

---

## Open questions (answer these before brainstorming)

1. ~~**mattpocock skills**~~ — Resolved: `mattpocock/skills`, cloned at `new-workflow/skills/`.

2. ~~**ui-ux-pro-max-skill**~~ — Resolved: `nextlevelbuilder/ui-ux-pro-max-skill`, cloned at `new-workflow/ui-ux-pro-max-skill/`.

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
- **Context explosion**: ~90-100K tokens consumed at session start. → **Root-caused 2026-07-01, see "Pivot execution" below.** It is NOT the superpowers `hooks/session-start` hook (only ~860 tokens, fires on start/compact only — confirmed negligible). It IS root `CLAUDE.md`'s mandatory chain of 6 full skill bodies (~13,250 tokens) plus mandatory doc reads, forced every milestone. Fix is loosening `CLAUDE.md`'s workflow rigidity, not hook edits.

### New skills needed (not in Superpowers)

- **db-design guide**: Brainstorm complete (see `temp/refs/db-design-skill/db-design-guide-notes.md`). Base: agent-toolkit Skill-4, modified. Ready to write any session.
- **debug guide**: Core principle — agent must not state a cause without proof. "Hypothesis: X might cause this → verify by doing Y." Must replace the assumption-heavy debugging behavior.
- **backend-design guide** (name TBD): Domain/service organization, route hierarchy, API naming. Covers Delapse Issues 4, 6, 7. Needs its own brainstorm session.
- **brainstorm guide**: Draft exists at `framework-build/docs/guides/core/brainstorm.md` — treat as scratch only. Needs full redesign session.
- **execute/handoff/verify guides**: Still pending.

---

## Superpowers skills inventory

| Skill | Keep / Modify / Remove | Git-mutation audit (2026-07-01) |
|---|---|---|
| `brainstorming` | **Replace wholesale** (decided 2026-07-01 session 2) — not a line-edit. New mechanic = framework-build's branch-tree design (`framework-build/docs/design-session.md` D27-29, D45-46): Phase 1 Open → Phase 2 Walk (one branch at a time, agent commits to a recommended answer, user reacts) → Phase 3 Assumption-check (now a global cross-cutting rule — see Skill-by-skill audit section below, NOT brainstorming-specific) → Phase 4 Close. `brainstorm.md` = living working-memory doc; exact location TBD (depends on new-workflow's eventual milestone/session structure — still open). HARD-GATE removed → soft norm only ("don't start implementing before the design is roughly agreed"). Visual companion section removed → replaced by the merged `visualization` skill (see below). Spec-writing stays a separate, real, mandatory step for implementation work (brainstorm → spec → plan → execute, always, not optional/foldable) — needs its own `write-spec` skill mirroring framework-build's design (reads brainstorm.md + conversation, synthesizes spec.md, no interview). **Finalize LAST**, after write-spec, context-capture, assumption-check, and visualization are all designed — brainstorming references all of them. | Strip "write doc + commit to git" step (superseded by the wholesale-replace decision above) |
| `writing-plans` | Keep, modify | Strip literal "Step 5: Commit" (`git add`+`git commit`) baked into generated plan templates |
| `executing-plans` | Keep or modify — needs separate brainstorm to assess | No mutation — only references `using-git-worktrees` (a dependency reference, not a mutation itself) |
| `systematic-debugging` | Modify — add proof-required rule | No mutation — only read-only `git diff`/recent-commits mention |
| `verification-before-completion` | Likely keep — assess | No mutation — descriptive text only |
| `requesting-code-review` | Likely keep — assess | No mutation — read-only `git rev-parse`/`git log` for SHA lookups |
| `finishing-a-development-branch` | **Deleted (session 5).** Entirely git/worktree operations; only non-git piece (run tests before finishing) is covered by `verification-before-completion`. | Real offender — deleted |
| `subagent-driven-development` | **Delete entirely.** (User: "really annoying.") | Whole skill being removed |
| `test-driven-development` | Remove (D3: no TDD) | No mutation — descriptive only |
| `using-git-worktrees` | **Delete entirely.** Confirmed: don't need anything about git for this. | Whole skill being removed — its native-tool-fallback path ran real `git worktree add`/`git branch` mutations |
| `using-superpowers` | **Confirmed 2026-07-01 (session 2): Modify.** Soften/remove lines 10-16 (`<EXTREMELY-IMPORTANT>` "1% chance...ABSOLUTELY MUST...not negotiable" block) → replace with "invoke when it clearly applies." Also drop the Red Flags table and the forced-invocation `digraph` flowchart further down the file — same over-invocation doctrine reinforced twice more. Keep: Instruction Priority section, How to Access Skills section — mechanical/harmless, no change needed. | No mutation — one descriptive example string only |
| `writing-skills` | Keep for reference | No mutation — one optional checklist item ("commit skill to git"), low risk |
| `receiving-code-review` | Likely keep — clean read, no conflicts with our rules (verified 2026-07-01) | No mutation |
| `dispatching-parallel-agents` | Likely keep — clean read, no conflicts (verified 2026-07-01) | No mutation |

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

## Pivot execution (2026-07-01)

Started executing the fork-fix list above, working directly in `new-workflow/superpowers/`. Mode for this whole effort: **plain conversational brainstorming only** — do NOT invoke the actual `superpowers:brainstorming` skill machinery (its HARD-GATE, forced spec doc, forced git commit, forced transition to `writing-plans`) for this meta-work. Ironic but deliberate: we're replacing that machinery, not running it on itself.

### Research findings (skill loading & token cost — use these, don't re-derive)

- **Skill loading is lazy everywhere**: only frontmatter (name + one-line description) loads automatically at session start, for plugin/project/user skills alike. Full `SKILL.md` body loads only on invocation. Confirmed via official docs (code.claude.com/docs).
- **No per-tool-call compounding.** Verified empirically by parsing a live session's own transcript JSONL: the full skill-catalog listing and the superpowers `using-superpowers` hook injection only re-fire on *discontinuity events* — session start, `/compact` (manual or auto), model switch (`/model`) — never on a per-tool-call basis.
- **The superpowers `hooks/session-start` hook is a red herring for token cost.** It's a plugin-level hook (not a skill — lives at `new-workflow/superpowers/hooks/hooks.json` + `hooks/session-start`, separate from `skills/`), fires on `matcher: "startup|clear|compact"`, and re-injects the full `skills/using-superpowers/SKILL.md` body as `<EXTREMELY_IMPORTANT>` context. But that file is only 62 lines / ~860 tokens — negligible even fired repeatedly. **Decision: leave the hook mechanism alone, not worth editing for cost reasons.** The only real problem in that file is the behavioral language at lines 10-16 (the "1% chance → must invoke" / "not negotiable" block) — already captured in the inventory table above.
- **The real source of large (50k+) token burns: the mandatory skill-body cascade in the current root `CLAUDE.md` workflow table**, not any hook. Measured directly: the standard milestone chain (`brainstorming → writing-plans → subagent-driven-development → verification-before-completion → requesting-code-review → finishing-a-development-branch`) loads **~13,250 tokens of skill bodies alone** every time it runs in full (subagent-driven-development ~5,400 tok, brainstorming ~2,600 tok, writing-plans ~1,770 tok, finishing-a-development-branch ~1,700 tok, verification-before-completion ~1,050 tok, requesting-code-review ~700 tok). On top of that: mandatory session-start reads (`now.md`+`workflow-rules.md`+`conventions.md`+`commands.md`), plus the active milestone's `spec.md`/`plan.md`/`session.md` (substantial in a real project), plus brainstorming's own "explore project context" reads.
- **Architecture implication**: don't build per-milestone dynamic skill-loading/toggling — the problem it would solve (context cost scaling with installed skill count) is smaller and differently-shaped than feared, since frontmatter cost is negligible and reinjection is event-bounded, not call-bounded. The actual lever is (a) loosen root `CLAUDE.md` so it doesn't mandate a long fixed read-then-invoke-6-skills chain, and (b) keep total curated skill count reasonable across whatever plugins get merged in.

### Root `CLAUDE.md` direction (decided, not yet executed)

Loosen the rigid milestone-flow backbone. Superpowers (the fork) becomes **one plugin among several, invoked when needed** — not the mandatory spine of every session. Stop mandating the fixed read-then-chain-6-skills sequence at session start; make the milestone structure lighter/opt-in.

### Next actions list (2026-07-01, original) — PARTIALLY SUPERSEDED, see below

1. Delete `new-workflow/superpowers/skills/subagent-driven-development/` entirely. — still pending
2. Delete `new-workflow/superpowers/skills/using-git-worktrees/` entirely. — still pending
3. Decide delete-vs-strip on `finishing-a-development-branch`, then execute. — still pending
4. Strip `writing-plans`' Step 5 commit block. — still pending
5. ~~Strip `brainstorming`'s commit-to-git step.~~ — superseded: brainstorming is being replaced wholesale, not line-edited (see below)
6. Check for + clean up dangling references to deleted skills in the remaining ones (e.g. `executing-plans/SKILL.md:68` and `subagent-driven-development/SKILL.md:409` reference `using-git-worktrees`; moot for the file being deleted, but check survivors). — still pending
7. Then: rewrite root `CLAUDE.md` per the "Root CLAUDE.md direction" above. — still pending

---

## Skill-by-skill audit + architecture decisions (2026-07-01, session 2)

Mid-session-1 we paused the mechanical git-mutation-strip walk to go skill-by-skill, file-by-file through the whole fork before touching anything, per explicit request ("I don't want you to immediately jump into action... go skill by skill, file by file, plugin by plugin"). This surfaced bigger architecture decisions beyond simple line-edits. Recorded here in full before compaction.

### Audit walk so far (in bootstrap order)

1. **`using-superpowers`** — confirmed Modify (see inventory table above). Locked.
2. **`brainstorming`** — confirmed Replace wholesale (see inventory table above). Locked at the architecture level; exact file content NOT yet drafted. Depends on write-spec, context-capture, assumption-check, and visualization all being designed first — finalize last.
3. Skills 3-14 (writing-plans, executing-plans, finishing-a-development-branch, subagent-driven-development, test-driven-development, using-git-worktrees, systematic-debugging, verification-before-completion, requesting-code-review, receiving-code-review, dispatching-parallel-agents, writing-skills) — **paused, not yet walked individually this session.** `receiving-code-review` and `dispatching-parallel-agents` got a clean read earlier (both fine, no changes needed — see inventory table). The rest still have only the original git-mutation-audit pass from session 1 (see inventory table) — need the same close read the first two skills got.

### New requirement: publish-ready changelog

User is considering publishing this fork publicly. Every deviation from upstream Superpowers needs to be documented with rationale as we go, not reconstructed at the end. **Decision: `new-workflow/superpowers/CHANGELOG.md`**, living in the fork repo root (OSS-idiomatic, travels with it if published). **Not yet created** — needs to be started and backfilled with the `using-superpowers` decision once we do the actual file edit (we've only agreed scope so far, not written the edit).

### Assumption-check — global hard rule, not a brainstorming phase

Corrected understanding (was wrong earlier in session): this is NOT specific to brainstorming. It's a hard rule that applies everywhere — anywhere the agent is operating on an unverified premise. The actual mechanism (how to detect/surface assumptions consistently across all phases, how many to surface, when it's noise vs. signal) is a genuinely hard problem. **Needs its own dedicated brainstorming session — not designed yet, do not improvise a mechanism.** Listed as a sub-skill to design before finalizing `brainstorming`.

### Context-capture — new cross-cutting skill, NOT yet built

Problem statement (from user): `brainstorm.md` solves note-capture during brainstorming, but spec-writing/planning/execution/research/debugging phases also surface things worth keeping — and currently there's no consistent mechanism for that. Some notes are milestone/task-specific; some are global/project-wide (e.g. a future-feature idea that surfaces mid-brainstorm on something unrelated) and need to land somewhere different so they aren't lost or miscategorized.

**This is already solved in framework-build's locked decisions** — `framework-build/docs/design-session.md` D30 + D31:
- **D30 — three-layer capture:**
  - Layer 1, phase-specific working memory: `brainstorm.md` during brainstorming, code-state notes during execution, findings during research.
  - Layer 2, durable knowledge, written **immediately** when it surfaces: domain/stack gotchas → `docs/kb/<domain>/<topic>.md`; global future ideas → `backlog.md`; project-level architectural calls → `decisions.md`.
  - Layer 3, session checkpoint: `session.md` at natural breakpoints and session end.
- **D31 — immediate capture rule:** write the moment something surfaces, not at session end (session end is too late — compaction or abrupt end loses the intent to save). Brief inline note, then keep working. User reviews/edits later.

This maps directly onto the "global vs. milestone-specific" split the user described — Layer 2 (KB/backlog/decisions.md) is the global side, Layer 1 (brainstorm.md/session.md) is the milestone-specific side.

**Recommendation (stated, not yet locked):** build this as its own standalone, cross-cutting skill in new-workflow — tentatively named `context-capture` — usable from any phase, not folded into brainstorming. **Not yet designed or built.** Listed as a sub-skill to design before finalizing `brainstorming`.

### Scope reframe: new-workflow ≈ framework-build v2

User clarified (see updated scope note at top of this file): this isn't strictly "fork Superpowers, keep framework-build separately paused." It's the same underlying effort, continued with a different method — adapting/curating existing Claude/Codex-compatible skill ecosystems instead of writing a guide-system from scratch. framework-build itself may be entirely rewritten or dropped. Its 85 locked decisions are a validated reference to pull from freely. New skills designed in new-workflow (context-capture, assumption-check) may get ported back into framework-build later if it resumes. Still: only *write* to `new-workflow/` files unless told otherwise.

### Visualization — merge `diagrams` (new) + `visualization` (old) into ONE skill

New skill discovered: `new-workflow/new-skills/diagrams/SKILL.md` + `references/advanced-patterns.md` + `references/complex-diagrams.md` + `examples/` (7 files — see below). Old skill: `framework-build/docs/guides/core/visualization/GUIDE.md` (+ `html-sketch.html` template, not yet re-read this session but referenced in design-session.md D81-85).

**Decision: ONE merged `visualization` skill, not two separate/gated skills.** Multiple format options living inside it (text formats, SVG diagrams, HTML UI mockups) that the agent picks between per situation — corrected from an earlier wrong framing (this agent initially proposed a rigid two-tier gate between them, which the user explicitly rejected: "we are definitely going to merge the visualization and new diagram skill into a single visualization skill").

**What the new `diagrams` skill provides (high quality, verified by reading SKILL.md + both reference files in full):**
- Self-contained HTML/SVG output, no external deps, opens in any browser, light/dark via CSS custom-property tokens.
- Box-width formula from text length, computed viewBox (not guessed), hard spacing/arrow-routing rules.
- Intent-based decision tree: routes on the verb the user used ("what's the flow" → flowchart, "what's the architecture" → structural/containment, "compare X vs Y" → side-by-side comparison columns, "how does X work" → illustrative/spatial-metaphor diagram).
- Progressive disclosure: `advanced-patterns.md` (comparison columns, status-coded success/fail paths, return/loop arrows, callouts, legends) and `complex-diagrams.md` (12+ node diagrams, lane/backbone planning, multi-way branch/merge, parallel background branches, self-loops, when to split into overview + detail diagrams) only get read when the diagram actually needs that complexity — good token economy, same principle as everything else in this effort.
- Judged: effectively a strict upgrade over the old guide's HTML-sketch section, which had layout principles but no concrete geometry/formulas.

**Corrections made by user to this agent's first-pass assessment (important, don't repeat these mistakes):**
1. **Not two gated tiers — one skill, multiple options.** (See above.)
2. **UI mockups:** user believes the new `diagrams` skill can likely already handle UI-component-state comparisons fine based on their own experience (not actually a gap requiring a separate tool) — but keep the old `html-sketch.html`/GUIDE.md approach available as a fallback option inside the merged skill for safety, rather than assuming full replacement untested.
3. **Mermaid policy:** old guide bans Mermaid outright. Reasons (from user, corrected from this agent's guess): (a) Mermaid needs a separate markdown-rendering step — friction; (b) Mermaid output is frequently confusing/unclear compared to the new skill's flows — NOT a claim about SVG having problems. The `diagrams/SKILL.md` decision tree (line 97) does carve out one exception — "draw the schema/ERD → use Mermaid.js via CDN, not hand-drawn SVG, table layout by hand fails every time" — that line is in the actual file, not this agent's invention, but the user is skeptical of it (doesn't recall SVG issues, the skill already leans on SVG successfully elsewhere). **Leaning: drop the Mermaid exception entirely, pure SVG/HTML for everything including ERDs** — the user's two stated objections to Mermaid apply equally to the ERD case. Revisit only if a real case proves SVG genuinely fails for ERDs specifically. **Not finalized — confirm before editing.**
   **Update (session 3, battle-test) — the case predicted here actually happened, see "ERD recipe — battle-tested and revised" below: dropping the exception was wrong, but Mermaid wasn't the final answer either. Landed on DBML + user's own `dbdiagram` VS Code extension.**
4. **Size/complexity constraints need review.** The `diagrams` skill was originally built for Claude.ai's web inline-diagram tool, which renders in a small embedded frame — hence the fixed `viewBox="0 0 680 H"`, "max 4-5 boxes per linear flow," "past 25-30 nodes, split into overview + detail diagrams," etc. In new-workflow's actual use case (a standalone HTML file opened in a real browser), there's no small-frame constraint — diagrams can be much larger and more flexible. **Action: review and likely loosen these size/complexity rules** before finalizing the merged skill — not yet done.
5. **Examples set needs review — not yet done by this agent.** User flags:
   - `examples/complex-system-example.html` is currently the file referenced as "the worked example" in `complex-diagrams.md`, but per the user it is **not** actually the most complex/best example available.
   - `examples/youtube_viral_loop_options.svg` is the most complex example AND the most visually clear — should likely become the primary referenced advanced worked example instead (or in addition).
   - `examples/outreach_agent_architecture.svg` — flagged as important to keep: good example of nested/wrapped containment across different environments ("wrapping notes inside different notes").
   - `examples/reply_relevance_scoring_fan.svg` — flagged as important to keep: another complex, unique example.
   - User suspects there may be too many examples overall (7 total: comparison-example.html, complex-system-example.html, emoji_mosaic_rendering_pipeline.svg, funding_credit_stack_flow.svg, outreach_agent_architecture.svg, reply_relevance_scoring_fan.svg, youtube_viral_loop_options.svg) and some could be trimmed/merged — but the three named above are explicitly worth keeping/featuring.
   - **Next action on resume: actually open and read `youtube_viral_loop_options.svg`, `outreach_agent_architecture.svg`, `reply_relevance_scoring_fan.svg`, and `complex-system-example.html`, assess them, decide example-set curation, and decide whether `complex-diagrams.md`'s "worked example" reference should change.** Not done yet — deferred to conserve context right before this compaction.

### Visualization — merge complete (2026-07-01, session 3)

All open items resolved and executed:

- **Examples curated**: read all 7 example files, dropped 3 redundant ones (`comparison-example.html` — subsumed by `youtube_viral_loop_options.svg`, which does the same comparison+status-coded+recommendation pattern with real content and a mask technique the synthetic one lacks; `funding_credit_stack_flow.svg` and `emoji_mosaic_rendering_pipeline.svg` — both taught nothing `complex-system-example.html` doesn't already cover). User deleted the three files themselves. 4 remain: `complex-system-example.html` (stays as `complex-diagrams.md`'s worked example, unchanged), `youtube_viral_loop_options.svg`, `outreach_agent_architecture.svg`, `reply_relevance_scoring_fan.svg`.
- **Mermaid/ERD exception dropped**: removed the one "use Mermaid for ERDs" carve-out from the decision tree. Replaced with a real `Recipe: entity-relationship (schema)` section in `SKILL.md` — table boxes (header + column rows, `PK`/`FK` tags) connected by cardinality-labeled lines instead of hand-drawn crow's-foot notation. Pure SVG now, no exceptions.
- **Size/frame constraints loosened**: root cause was `viewBox="0 0 680 H"` — width was hardcoded, tuned for Claude.ai's small embedded diagram panel; only height was ever computed. Fixed to `viewBox="0 0 W H"`, both computed from content, mirroring the pattern the skill already used for height. Cascaded through 4 other spots that justified their limits by citing the 680 number (spacing arithmetic, structural-nesting depth, comparison-column x-ranges, complex-diagram lane assignments) — all reworded to be content-driven, not frame-driven. Left the genuinely legibility-based rules alone (max 4-5 boxes/flow, 12-20/25-30 node budget, box typography) — those aren't frame artifacts.
- **Merged into one skill**, structure confirmed by user before writing: `new-workflow/new-skills/diagrams/` renamed to `new-workflow/new-skills/visualization/`. `SKILL.md` gained a **Step 0** (does this need a visual at all — ported from old GUIDE.md's escalation-gate + working-process rules) ahead of the existing decision tree, which became **Step 1** and gained a new top-level **UI mockup** row (routes to `references/ui-mockups.md`, HTML/CSS not SVG — applies to any UI markup request, not just comparisons, per user correction). Two new reference files ported from `framework-build/docs/guides/core/visualization/GUIDE.md`: `references/text-formats.md` (text-tree/numbered-prose/sequential-flow, No-Mermaid rule, telegraph style) and `references/ui-mockups.md` (layout principles, one-shot rules, explicitly marked as "default today, not a permanent lock" per user's note that the diagram machinery may eventually absorb mockup generation too). `advanced-patterns.md` and `complex-diagrams.md` carried over unchanged.
- **Palette unified**: user corrected an earlier "battle-tested, keep separate" framing from this agent — old HTML-mockup palette wasn't actually battle-tested, just visually spot-checked. Decision: **diagram system's CSS-custom-property tokens win everywhere; whenever old-mockup and new-diagram conflict, new-diagram wins** — because it empirically produces better results. `examples/html-sketch.html` copied over from `framework-build` and repaletted onto the diagram system's exact tokens (dropped `--text-faint`/`--border-inner`, which the diagram system doesn't have, rather than inventing replacements — collapsed into `--text-muted`/`--border`). Kept the manual light/dark toggle mechanism (JS class-swap) since it's an orthogonal UX affordance, not a color-system conflict — just wired to the same hex values as the diagram system's `prefers-color-scheme: dark` block.

Final file tree:
```
new-workflow/new-skills/visualization/
  SKILL.md
  references/{text-formats,ui-mockups,advanced-patterns,complex-diagrams}.md
  examples/{complex-system-example.html, html-sketch.html, outreach_agent_architecture.svg, reply_relevance_scoring_fan.svg, youtube_viral_loop_options.svg}
```

`framework-build/` source files were read, not edited — consistent with the read-only rule.

### ERD recipe — battle-tested and revised (2026-07-01, session 3 continued)

Battle-tested the merged `visualization` skill by actually generating and rendering diagrams (CI/CD flowchart, ERD schema), not just reading the skill. Flowchart recipe held up under real stress (17-node diagram, one trivial fix). ERD recipe did not — this is the case point 3 above said to watch for.

- **Round 1:** hand-rolled SVG table-boxes-and-lines recipe (the one written when the Mermaid exception was dropped) produced a schema diagram with no arrowheads on relationship lines, lines running straight through column text, and a line clipping back through its own source table's interior. Confirmed by screenshot, not just code review.
- **Round 2:** put the Mermaid exception back (`erDiagram`, cardinality tokens, save as `.md` fenced block). Structurally correct — real crow's-foot notation, no crossing lines, auto-routed curves — but visually plain/generic. User independently confirmed this with a separate Claude session producing similarly mediocre Mermaid ERD output.
- **Round 3 (settled, and written into `SKILL.md`):** schema/ERD diagrams use **DBML** as the artifact, rendered by the user's own **`dbdiagram` VS Code extension** (already installed — real-time local preview, no login needed for basic rendering, no copy-paste to a website). User confirmed the rendered output was good. `SKILL.md`'s "Recipe: entity-relationship (schema)" section, its Step 1 table row, the frontmatter description, and the "Output workflow (SVG diagrams)" line all updated to say DBML instead of Mermaid/SVG. `text-formats.md`'s "No Mermaid" footnote simplified back to a flat statement since there's no Mermaid usage left anywhere in this skill to carve an exception for.
- Test artifact: `temp/viz-test-4-erd-schema.dbml` (5-table schema: organizations/projects/users/tasks/comments — same one used for all three rounds).

**Two new hard rules from this same session (apply everywhere, not just visualization):**
1. **Never write scratch/temp files to root `/tmp`.** Use an in-repo temp location the user can actually browse (this repo's existing `temp/` at repo root, or a folder inside whatever `new-workflow/` area is relevant) — root `/tmp` paths are deeply nested per-session sandbox dirs the user can't easily find or read, and that's a real annoyance, not a style preference.
2. **Never install extensions/tools on the user's behalf.** If a VS Code extension or similar is needed, tell the user what's needed and let them install it (they frequently already have it installed).

### SKILL.md split into reference files (2026-07-01, session 3 continued)

User flagged that `SKILL.md` had grown into both a dispatcher and a reference manual (237 lines) — asked whether things like the base stylesheet belonged in the main file given text-formats will be the most common path and everything else is secondary. Agreed and split:
- `references/svg-diagrams.md` (new) — base stylesheet, arrow marker, all hard rules, box anatomy, the flowchart/structural/illustrative recipes, "Scripting large diagrams," and an SVG-specific done-checklist. Everything shared by the 3 SVG-based recipes, none of it needed for Step 0/Step 1 routing.
- `references/entity-relationship.md` (new) — the DBML recipe, moved out wholesale. It never touched the SVG stylesheet/hard-rules machinery anyway, and this now matches how `ui-mockups.md` already handles its own non-SVG recipe.
- `SKILL.md` — cut to 60 lines: frontmatter, Step 0, Step 1, a short "Output workflow" section that just points at the reference files, and a short generic checklist (detailed SVG checks moved into `svg-diagrams.md`).
- Fixed stale cross-references that pointed at the old inline location: `references/ui-mockups.md`, `references/complex-diagrams.md`, `examples/html-sketch.html`.

### Battle-test round 2 — comparison, UI mockup, illustrative (2026-07-02)

Finished the battle-test list left pending after the ERD/SKILL.md-split work: generated and rendered real diagrams for the three remaining families (flowchart and ERD were already covered in round 1). All test artifacts live in `temp/viz-test-5/6/7-*` (continuing the numbering from round 1's `viz-test-1..4`), not the repo root or the tool's `/tmp` sandbox.

- **Comparison** (`temp/viz-test-5-comparison-grill-skills.html`) — grill-me vs grill-with-docs, real project content. 2 columns, one asymmetric dead-end branch (coral, "not ready yet → use grill-me instead"), two teal success terminals, one insight callout, one legend. Rendered clean on first pass — no fixes needed.
- **UI mockup** (`temp/viz-test-6-milestone-dashboard-mockup.html`) — a milestone-status card in 3 states (idle / in-progress / ready-to-ship), adapted from `examples/html-sketch.html`. Verified in both light and dark via the built-in toggle. Clean on first pass.
- **Illustrative** (`temp/viz-test-7-progressive-disclosure-illustrative.html`) — spatial metaphor for the skill's own progressive-disclosure design: SKILL.md as an iceberg tip (always read, warm/amber), reference files as submerged blobs (cool/gray = dormant), one lit up amber + connected by a light-beam for the scenario "user asks to draw the schema" → `entity-relationship.md`. Uses the one permitted `linearGradient` for water depth. Clean on first pass.

**One real bug found and fixed:** `references/svg-diagrams.md`'s hard rules never said to add `width="100%"` to the `<svg>` tag — only `viewBox`. Without it, a browser renders the SVG at its default 300×150 intrinsic size and `viewBox` only rescales content inside that tiny box, so the diagram looks broken/invisible despite every coordinate being correct. The existing canonical example (`complex-system-example.html`) happened to do this right, but nothing in the recipe told a fresh reader to. Both new SVG test files were written without it initially, caught only because this agent happened to recall the pattern from the example file before rendering — a first-time reader following `svg-diagrams.md` literally would not know to. **Fixed:** added an explicit rule right after the ViewBox rule in `svg-diagrams.md`.

Two softer, unresolved observations (not fixed, flagged for later):
1. `advanced-patterns.md`'s comparison-badge guidance (red=breaks/green=works/gray-blue=neutral) doesn't cover the case where neither option is objectively better (situational choice, like grill-me vs grill-with-docs) — worked fine by treating blue+gray as "two neutral tones" and reserving status color for branches, not badges, but the doc doesn't say this explicitly.
2. The "dead-end termination" pattern (dashed line trailing into nothing) and "status-coded path" pattern (solid line, colored by outcome) aren't clearly distinguished for the case where a divergent branch *does* land in a real, labeled terminal box (not just trailing off) — had to judgment-call solid+coral for this test's "not ready yet" stub.

All 7 items on the original battle-test task list are now done: flowchart, ERD, comparison, UI mockup, illustrative, playwright render pass, findings report.

### Work order agreed this session

Build sub-skills first, in roughly this order, then come back to finalize `brainstorming` (since it depends on all of them):
1. ~~**Visualization merge**~~ — **DONE (session 3)**, see "Visualization — merge complete" above.
2. ~~**Context-capture skill**~~ — **DONE (session 4)**. Skill at `new-workflow/new-skills/context-capture/SKILL.md`. Key decisions: passive + user-invocable; open-ended triggers (agent judgment, not a fixed checklist); defaults table (decisions/notes/backlog/preferences/session.md); adaptive checkpoint format; hard rule that project info goes in repo not auto memory; lazy file creation; defers to existing project structure.
3. ~~**Assumption-check rule**~~ — **DONE (session 5)**. Saved to `new-workflow/hard-rules.md` (staging area for always-active rules that will go into the new-workflow's eventual CLAUDE.md). Rule: separate observations from inferred causes; every causal claim must be labeled as a hypothesis with a verification step ("Hypothesis: X. To verify: Y.").
4. **`write-spec` skill** — **DRAFT EXISTS, needs proper brainstorm + rewrite.** A draft was written prematurely at `new-workflow/new-skills/write-spec/SKILL.md` before the brainstorm was completed. Brainstorm was in progress (first question posed: does write-spec require brainstorm.md to exist, or can it work from conversation context alone?) when compaction hit. Resume the brainstorm from scratch — discard the draft, design properly, then rewrite. Key inputs already gathered: Superpowers brainstorming/SKILL.md spec section (self-review checklist, user review gate) and spec-document-reviewer-prompt.md (Completeness/Consistency/Clarity/Scope/YAGNI). **Next up.**
5. ~~**`brainstorming` skill**~~ — **DONE (session 5)**. Fully rewritten at `new-workflow/new-skills/brainstorming/SKILL.md`. Claude Code only (no web references), full decision capture per branch (not one-liners), grilling principles folded into Phase 2 (relentless, codebase-first, dependency-ordered walk, probe vague responses), Phase 4 hands off to write-spec. Hard rule added: never print tree + walk first branch in same message.
6. Resume the original mechanical git-mutation-strip walk for the remaining superpowers skills (writing-plans, executing-plans, finishing-a-development-branch, subagent-driven-development, using-git-worktrees, test-driven-development, systematic-debugging, verification-before-completion, requesting-code-review, writing-skills) — paused mid-walk, can resume any time, doesn't block the architecture work above.
7. `new-workflow/superpowers/CHANGELOG.md` — start this and backfill as each decision above gets actually executed (not just agreed).
8. Rewrite root `CLAUDE.md` per "Root CLAUDE.md direction" (still valid, unchanged from session 1).
9. Still-open, non-blocking: plugin final location (new repo vs. folder vs. elsewhere), install target (replace Superpowers entirely vs. sit alongside).

### agentic-workflow_v2 — template structure + executing-plans (2026-07-14, sessions 6–7)

**Pivot: primary deliverable is now `agentic-workflow_v2/`**, a self-contained starter-repo template, not a collection under `new-workflow/`. Skills live in `agentic-workflow_v2/.claude/skills/`. The `new-workflow/new-skills/` folder was deleted (user deleted after confirming all skills copied to v2).

**Skills in `agentic-workflow_v2/.claude/skills/` (all built):**
- `brainstorming/` — SKILL.md + write-spec.md
- `context-capture/` — SKILL.md
- `research/` — SKILL.md
- `research-evaluation/` — SKILL.md
- `visualization/` — SKILL.md + references/ + examples/
- `writing-plans/` — SKILL.md (updated this session — see below)
- `executing-plans/` — SKILL.md (new this session)

**Other files in `agentic-workflow_v2/`:**
- `.claude/settings.json` — git mutation deny rules
- `.claude/agents/haiku-worker.md` — static Haiku subagent instructions (new this session)
- `scripts/tree.sh` + `scripts/merge-files.js` — utility scripts (merge-files extended this session with `:N-M` line range syntax)
- `CLAUDE.md` — project template (updated with batch-checks rule + merge-files/parallel-read rule)
- `recommended-tools.md` — catalog of optional external skills
- `docs/work/now.md` + `docs/work/roadmap.md` — stubs

**Executing-plans skill decisions (extensive brainstorm, now built):**

- **Two modes:** Delegate mode (default) and Inline mode
- **Delegate mode:** Haiku subagent executes tasks with exact code specified. Main agent dispatches with ~40-token Agent call (references files by path, does NOT generate brief content inline)
- **Brief construction:** Main agent passes `offset`/`limit` to point Haiku at exact plan section. Haiku reads `.claude/agents/haiku-worker.md` + plan section + ≤3 source files. Main agent Sonnet output per dispatch: ~40 tokens
- **Haiku file read budget:** soft limit 5 total (2 for instructions + plan, 3 for source files)
- **Task classification:** DELEGATE = step includes complete exact code. INLINE = requires discovery/judgment. Plan marks INLINE tasks with `<!-- INLINE -->` comment
- **Haiku error handling:** obvious fix → one attempt, then report. Non-obvious → NEEDS_DEBUG immediately, no guessing
- **Debug agent:** Sonnet, `run_in_background=True` (FleetView visible + interactive). Receives full context brief. User notified when spawned
- **Verification:** always one chained Bash call with `&&` (never separate calls) — hard rule in CLAUDE.md
- **Progress tracking:** Edit plan.md to mark `[x]` after each verified task

**merge-files.js extension:** Added `:N-M` line range syntax to path args (e.g., `plan.md:45-89`). Ranged entries skip the `--ext`/`--except` filters (explicit inclusion). Opener label shows `file:N-M` to indicate slice.

**CLAUDE.md additions:**
- `Batch sequential checks` hard rule
- Parallel Read (≤4 files) vs merge-files (5+) guidance
- merge-files `:N-M` syntax documented

**writing-plans update:**
- New "Delegate-Ready Tasks" section: tasks must include exact code, exact file paths, explicit verification command
- Tasks needing discovery marked `<!-- INLINE -->`
- Plan header updated to reference delegate/inline distinction

**Study case analyzed:** `temp/study-cases/handy-workspaces/` — SDD-driven execution of a 9-task feature. Root failure: ~1.1M tokens, 90 min for a feature that should have taken 15 min. Caused by per-task implement+review subagents (each starting cold), final whole-branch review (90k tokens), mega fix-agent (124k tokens). Informed the entire executing-plans design.

---

## On resume

**Current state: `agentic-workflow_v2/` is the primary artifact.** Skills built (7): brainstorming (+write-spec.md), writing-plans, executing-plans, context-capture, research, research-evaluation, visualization.

### ACTIVE THREAD (2026-07-17, session 9) — Skill & knowledge ecosystem (#7)

**The current live design work is `new-workflow/design-skill-ecosystem.md` — read it first on resume.** It designs the reusable, growing, publishable knowledge system that sits alongside v2's process skills (the "I always lose/miss conventions" problem). Grounded in the user's own reference implementation `temp/debug-web-pages/`.

**Corrected core model (confirmed):** the **skill is the atomic unit** of reusable/growing/publishable knowledge; its internal `knowledge/` folder IS the cross-project knowledge base (no separate store). Reusable → a skill; the rare project-only thing → project docs (`docs/spec/decisions`). Workflow and skills are separate publishable artifacts.

**Branches CLOSED (#1–#4) — full detail in the ecosystem doc's LOCKED sections:**
- **#1 — skill anatomy standard.** Thesis: *standardize the NAMES of the pieces, not WHICH pieces a skill has.* One hard invariant = `SKILL.md`; everything else (`knowledge/`, `tools/`, `MAINTAINING.md`) **optional-by-purpose, no thresholds.** Two heuristics: smallest-shape-that-works + load-frequency split (`SKILL.md` lean+stable+always-loaded / `knowledge/` on-demand). `DESIGN.md`/`ROADMAP.md` = WIP-only scratch, NOT anatomy; only `MAINTAINING.md` stands. Knowledge layout **emergent** (shape-library: instance-cache / topic / recipe / flat-note — OPEN, not a fixed taxonomy). `_TEMPLATE.md` scoped to instance-cache ONLY, NOT domain-agnostic. `debug-web-pages` is ONE shape, not the template.
- **#2 — growth/maintenance framework.** *Part A:* maintenance mechanics hoisted into ONE reusable skill **`maintaining-skills`** (NEW, two-mode: **Grow** = write/promote/prune; **Audit** = check current work against a skill's best-practices). Per-skill `MAINTAINING.md` demoted to THIN config the meta-skill reads. Audit lives HERE, not in `verification-before-completion` (which isn't in v2 yet + is milestone-scoped; audit is skill-scoped/cross-cutting; future: verify MAY *call* audit but doesn't own it). *Part B (the janitor's 5 rules, plain-language):* 1 don't-write-twice→move-to-shared; 2 delete-junk-aggressively (git is the archive); 3 route-new-knowledge (general→shared / specific→that file / new-ability→tool + one line in `SKILL.md`); 4 write-only-confirmed (dated, sourced, stable-locator, guesses in "open questions"); 5 keep-notes-short (positive, single-source, leading-words).
- **#3 — capture/routing loop** (the "I miss things" fix). Split **Notice** (free — one line into a file during work, rides on `context-capture`) from **Sort** (later, at a stopping point). The reusable-vs-project question is NOT a real fork (nearly everything is reusable) → **flip the default → a skill**; rare exception → project docs. **NO global inbox** (rejected) — a note with no skill yet → create a local stub skill on the spot. Skills are **local + git-tracked in a clone you own** so edits are visible/reviewable.
- **#4 — publishing/packaging** (CLOSED 2026-07-18). **Load-bearing correction:** the pick-skills/pick-agent/pick-scope installer the user liked is `npx skills add` (**`vercel-labs/skills`**), NOT Claude Code's native plugin install (which is **all-or-nothing per plugin**, `/plugin marketplace add` → `/plugin install name@mkt`). A repo can satisfy both at once (mattpocock does). **Decisions:** (1) **two repos** — Repo A = workflow template (`agentic-workflow_v2`: CLAUDE.md + docs scaffold + the core "how to work" skills), Repo B = skills catalog (stack/tool knowledge skills, grows forever, installed piece-by-piece); (2) **no git submodules** — two clones side-by-side in one local folder, template references catalog by install-command/URL; (3) **core skills live INSIDE the template (Option 1)** — they're part of the workflow, version with CLAUDE.md, not external add-ons; catalog holds only stack/tool skills; (4) **link, never hard-copy** — symlink from a clone so `git pull` updates everywhere (copies are frozen; `--copy` stays available for strangers); (5) **ship both doors** — catalog is `npx skills`-installable AND carries `marketplace.json`; engine is a whole-bundle plugin; (6) **release discipline deferred** — no version numbers (Claude Code treats no-version as "every commit = latest"); add explicit versions + changesets only once strangers install.

**Global-repo model (mechanics now LOCKED in branch #4 above):** hosted install-FROM source, mattpocock-style (`new-workflow/skills` = the reference) — clone → `npx skills add` picker → land local (symlinked) → edit in the git-tracked clone (edits visible) → `git push` promotes worthwhile edits UP → others `git pull`/`skills update`.

**NEXT ACTION:** walk ecosystem-doc **branch #5 — init-integration seam** (at `project-init`: read the tech spec → recommend/import relevant skills, external + personal → scaffold project-local capture; touches genesis-doc #4/seeding). Then **#6** skill-creation trigger ("no skill yet → make one"; who authors it — `write-a-skill` / `superpowers:writing-skills`). That closes the ecosystem thread.

**Front-of-lifecycle thread (`design-project-genesis.md`) — essentially CLOSED this session:** locked 3.2 technical core (a engine / b what-"resolved"-means / c research-plug-in+`docs/research/` storage), 3.3 interaction (4-phase A–D walk), 3.4 working memory (`docs/work/consolidation.md` + bootstrap now.md + context-capture), renamed skill **project genesis → `project-init`**, closed #4 derivation (Phase E), closed **#8 now.md = thin cursor**. Only #6 skill-topology remains parked there.

**Mode reminder:** plain conversational brainstorming — do NOT invoke `superpowers:brainstorming` or the v2 `brainstorming` skill on this meta-work. Commit to a position per branch, one branch at a time. `framework-build/` + `temp/debug-web-pages/` are read-only reference; do NOT reference the deprecated v1 `project-init.md` (the new `project-init` is a fresh design reclaiming the name).

### Skills still to build (after the design threads close)
- **`debugging`** — mid-execution debug flow. Needs brainstorm. Framework-build D52-D53 has design notes (Phase 0 calibrate + 5 adaptations of diagnosing-bugs). Hard rule "never state cause without evidence" already in CLAUDE.md.
- **`verification`** / **`finishing-topic`** — needs brainstorm (may be 1 or 2 skills)
- **`project-init` skill** itself — build once the genesis design closes (design is essentially done; #6 topology parked).
- **Skill ecosystem** (#7) — design in progress (`design-skill-ecosystem.md`), branches #1–#3 closed; implementation after design closes.
- **`maintaining-skills` skill** (NEW, confirmed session 9) — the two-mode (Grow + Audit) reusable meta-skill that maintains other skills' knowledge; content = branch #2's 5 rules. Build after the ecosystem design closes.
- **Architecture skill — INVESTIGATE (low priority, 2026-07-18):** evaluate mattpocock's `codebase-design` vs the installed `improve-codebase-architecture`; decide adopt / reuse-one / build-own. Likely lands in the global skills catalog (Repo B), not the template. Surfaced while pruning `recommended-tools.md` — deliberately kept OUT of that file until decided. Not a commitment.
- **Plugin registration** — how to get `flow:` prefix working with local install. Deferred.
- **CLAUDE.md final content** — deferred until all skills exist. Note: v2 CLAUDE.md session-start wording will need a tweak for the #8 now.md thin-cursor (read now.md for active topic → *infer* next action from the topic folder).

**Note:** `write-spec` is NOT a missing skill — it already exists as Phase 4 of `brainstorming` (`skills/brainstorming/write-spec.md`). Earlier notes calling it a gap are outdated.

**Pending mechanical fix (awaiting go):** `agentic-workflow_v2/docs/work/roadmap.md` → `backlog.md` (framework-build D16: no roadmap, flat backlog). Also (from #3.2.c): a future `docs/research/` folder convention for external research reports (`<NN>-<slug>.md`, prompt+report together, referenced not inlined).

**Do NOT touch `framework-build/` files (read-only reference). Do NOT reference the deprecated v1 `project-init.md`.**

_Updated: 2026-07-17 (session 9)._
