# Design — Project Init (front-of-lifecycle)

> Skill name: **`project-init`** (renamed from "project genesis" — too fancy; reclaims the name from the deprecated v1 command it replaces). Phase working-name = **Consolidation** (internal, soft). "genesis" left in a couple of older notes below = the init record / an init run.

_Live brainstorm record. Started 2026-07-16. Meta-work on `agentic-workflow_v2/`._

Designing the **front of the v2 lifecycle**: how a user goes from a raw project idea (or an existing project) to an implementation-ready foundation. v2's middle (brainstorm → spec → plan → execute) is already built; this is the missing front end.

---

## Ground rules for this discussion

- **Plain conversational brainstorming only.** Do NOT invoke `superpowers:brainstorming` or the v2 `brainstorming` skill on this meta-work.
- **framework-build/ is read-only reference.** Its principles (D8, D9, D16, D23) are largely correct for this front-of-lifecycle and are the sound starting point — but adapt to v2, don't port verbatim.
- **v1 `project-init.md` is DEPRECATED** (root project's `.claude/commands/project-init.md` + `docs/agents/commands/project-init.md`). It fills v1-shaped files (docs/agents/conventions.md, commands.md, roadmap.md, milestones) that don't exist in v2. Do NOT reference it as a model.
- No file changes to deliverables without explicit approval.

---

## The core insight (the dependency I had backwards)

`docs/spec/` is the **foundation** and must be complete **first**. CLAUDE.md, now.md, backlog.md are all **derived from it** — name/stack/structure, the first thing to build, and the backlog items all come from the spec. You cannot write any of them before the spec folder exists.

Overall shape:

```
variable input  →  [ normalize / consolidation phase ]  →  canonical docs/spec/  →  derived scaffold
 (intake/)          synthesis + technical de-risking        base + emergent files    (CLAUDE.md, now.md, backlog.md)
```

Two entries into the SAME flow: **greenfield** (loose docs / an idea) and **migration** (existing repo + its docs). One adaptive skill, not two modes.

---

## LOCKED decisions

### Spec-first is mandatory (not opt-in)
For the initial version, the spec-folder path is **required**, following framework-build's approach. (This reverses an earlier "make it opt-in" idea — that's dropped.)

### #1 — Canonical target (`docs/spec/` structure)
- **Not a rigid template.** Stable **base = product, tech, decisions**.
- Each base file can **split into multiple** files — tech especially (`tech/stack.md`, `tech/architecture.md`, possibly several tech files).
- Plus **emergent additional types** as the material warrants: `open-questions`, `market-validation`, marketing reports, etc.
- The agent **proposes the final structure at the END of the normalize phase**, based on what's actually there. Base trio always present; everything else emergent.
- If the user brings **market-validation / marketing docs**, utilize AND preserve them.
- (Open, minor: whether to also keep original intake files inside spec/ — resolved by #2: originals stay in `intake/`.)

### #2 — Intake
- Dedicated **`docs/intake/`** folder. User pastes whatever they have there — idea, product bible, tech-stack file, market-validation reports, any mix.
- The normalize phase **reads `intake/`**, synthesizes canonical files into **`docs/spec/`**, and **leaves the originals in `intake/`** (preserved reference, never overwritten).
- **Rejected alternative:** pasting straight into `docs/spec/` and normalizing in place — loses the raw/canonical boundary and mangles originals.

### #4 — Derivation (spec → scaffold)
- Once `docs/spec/` is canonical, **generate** CLAUDE.md (name/stack/structure) + now.md (first thing to build) + backlog.md (future work) **from it**.
- Strictly downstream of the spec being ready.
- (Exact "is derivation a separate step or the phase's closing step" — treat as agreed-in-principle; mechanics TBD, low controversy.)

### #5 — Greenfield vs migration
- **Single adaptive flow.** No two modes. "Take whatever it is and make it work." Greenfield (loose docs) and migration (existing repo + docs) are just two entries into one flexible skill.

### #3 — The normalize phase — REFRAME + stop condition
- **NOT "gap-filling."** It's **synthesis + technical de-risking** → produces a **complete, validated, build-ready foundation**. Put the pieces together, pressure-test that it's practical, do the technical research and evaluations, until there's a **full product idea AND a full technical plan** you could start building from immediately.
- The gap-map (3.1) is the **starting diagnostic**, not the work itself.
- **Names (LOCKED):** skill = **`project-init`**; phase working-name = **Consolidation** (internal, soft).
- **STOP CONDITION (outcome-defined, not gap-defined):** done = a reader could pick up `docs/spec/` and **immediately (a) fill the backlog and (b) start implementing, with NO unresolved technical unknowns.** Higher bar than "documented all decisions" — feasibility is actually *resolved*, not just discussed.
- **Implication:** the phase does **real technical work** — the `research` skill, option evaluations, feasibility checks — not just conversation. Technical is the heart (user's emphasis).

### #3.2.a — Engine of the technical core (CONFIRMED)
- **Reuse the `brainstorming` loop mechanics** — walk a branch tree one at a time, propose→recommend→react→write, `research` before technical recommendations, `visualization` for architecture — as the engine of the technical de-risking work.
- The **3.1 gap-map IS the initial tree** (technical-critical first). Consolidation feeds it in and walks it; no new tree from scratch.
- **BUT Consolidation is its own project-altitude skill**, not a literal call to the milestone-scoped `brainstorming` skill. Key difference is **altitude + output target:** `brainstorming` runs at milestone/feature altitude → `docs/work/topics/<slug>/brainstorm.md` → `write-spec` → one topic `spec.md`. Consolidation runs at **project altitude** → canonical `docs/spec/` foundation (product/tech/decisions). Same loop pattern, different altitude and destination — the two must not bleed (a genesis run must never dump into `docs/work/topics/`).

### #3.2.b — What "resolved" means (CONFIRMED)
Operational definition of the stop condition's "no unresolved technical unknowns," given Consolidation writes docs, not code:
- **Instrument = research, not code spikes.** De-risk via `research` + `research-evaluation` (docs, option comparison, API-behavior checks, currency validation). **No code written in Consolidation** (breaking the project-altitude boundary = starting to implement). A gap is resolved when research confirms the approach is viable AND the specific integration path is known — concrete enough an implementer won't hit a "can this even work?" wall.
- **Depth governor = one-way vs two-way doors** (anti-gold-plating):
  - **One-way doors** (irreversible, high blast radius, cross-cutting — stack, core architecture, data model, load-bearing integrations) → **must be resolved now.**
  - **Two-way doors** (reversible, local, cheap to change) → **noted + deferred** to implementation time ("decide when building X"). Forcing these to closure now = over-engineering the spec.
  - Depth per gap = "research until an implementer wouldn't stall, no further." Gap-map's technical-critical-first ordering aims research at one-way doors first.
- **Escape hatch = code-only unknowns become a flagged first spike, not a silent gap.** When only running code can prove viability (e.g. throughput under a specific library), Consolidation does NOT fake resolution on paper. It records the risk explicitly in `docs/spec/` (decisions/open-questions), and the derived `now.md` makes the **first milestone a spike** to de-risk it.
- **Net:** "no unresolved technical unknowns" = every one-way door research-resolved, every two-way door consciously deferred, every code-only risk a flagged spike. Nothing load-bearing left to chance; reversible stuff not over-researched.

### #3.2.c — How `research` plugs in + storage split (CONFIRMED)
- **On-demand, per-gap, in dependency order** — same as `brainstorming` already does. Walk a gap, hit something whose answer needs current knowledge, invoke `research` for that gap, commit. Each invocation is informed by gaps already resolved above it (don't research realtime-sync libs before the stack is settled). Gap-map's technical-critical-first ordering drives this. **Not an up-front batch pass.**
  - *Exception:* batch adjacent gaps sharing one underlying question into a single broad/comparative external prompt. Default per-gap; batch by judgment.
- **Mode selection inherited from the `research` skill**, not overridden: single focused lookup → direct tools (Context7/web/codebase); multi-source synthesis → external prompt.
- **External-prompt research is the primary multi-session driver.** That mode hands prompts to the USER, who runs them on their LLMs and returns file paths — a natural, possibly-hours/days-long pause. The technical core's biggest session boundaries fall here → direct link into 3.4 (the loop must survive "user went off to run deep-research prompts, came back tomorrow").
- **Storage split (the key distinction — reference vs. save):**

  | Artifact | Saved where | Relationship to working doc / spec |
  |---|---|---|
  | External research **reports** (substantial, user-run) + their **input prompts** | Dedicated **`docs/research/`** folder, one file per research question, **prompt + report kept together** for provenance | **Referenced by path only — never inlined.** |
  | **Synthesis** (what we learned, direction, caveats) | Consolidation working doc → folded into `docs/spec/` | Lives in the doc — the distilled value that survives compaction. |
  | **Direct-tool lookups** (Context7 / quick web check) | No separate file (no external report artifact) | Synthesized **inline** in the working doc. |
  | **The decision** the research informed | Working doc → `docs/spec/decisions` | In the doc, with a **path-reference** to the `docs/research/` report as evidence. |

  Rule: heavy external reports → files in `docs/research/`, referenced; light direct lookups → inline; only synthesis + decision live in the doc/spec — never the raw report body.
- **Folder placement:** `docs/research/` sits **parallel to `docs/intake/` and `docs/spec/`**. intake = raw material the user pasted (read-only); research = evidence generated during Consolidation (durable); spec = canonical foundation referencing both.
- **NOT one unified project-wide store.** `docs/research/` holds **Consolidation (project-altitude)** reports. Later, **topics/milestones get their own nested research folders** (e.g. under the topic folder) — research storage mirrors altitude, same as the working docs do.
- **Naming:** `<NN>-<slug>.md` — simple index + slug, **mainly the slug**. **No date prefix** (overkill). E.g. `01-realtime-sync-options.md`.
- **`research-evaluation` is explicitly OUT OF SCOPE for this design.** It's personal/unpublished, exists only to gather LLM-performance data for the user's own external-LLM ranking, and will be **deleted later**. It plays no part in Consolidation. Do not reference it here.

### #3.3 — Interaction half / phase structure (CONFIRMED)
Consolidation runs as an explicit 4-phase walk at project altitude:
- **Phase A — Assess** *(= 3.1):* read `intake/` by content → score dimension checklist → gap-map (per-dim status + ordered gap list, technical-critical first) → present "what I think you're building / solid / missing / order" → **user confirms/corrects before any gap-work.**
- **Phase B — Walk the gaps:** dependency-ordered, one branch at a time, propose→react→write into the working doc. **Interaction mode switches by the gap's dimension tag** (the main new idea in 3.3 — Consolidation is NOT uniformly "research everything"):
  - **Technical gaps → research-led** *(= 3.2)*: research resolves; one-way doors closed, two-way deferred, code-only → flagged spike.
  - **Product gaps → interview-led:** resolved by discussion, not research. Agent proposes a position on the product question, user reacts (e.g. "V1 scope is fuzzy — I'd pull X/Y out because they depend on Z; agree?"). Less lookup, more elicitation.
  - **Contextual gaps → captured only if the user has material; never forced.**
- **Phase C — Propose structure:** once the walk closes with no unresolved one-way doors, agent proposes the final `docs/spec/` layout (base trio always + emergent files the material warrants) → **user confirms the shape BEFORE any file is written** (explicit, not implicit-while-writing).
- **Phase D — Write spec:** write `docs/spec/` files from working doc + research synthesis, progressively, self-review (no placeholders, internal consistency), user gate.
- → hands off to **#4 derivation** (CLAUDE.md / now.md / backlog.md generated from the finished spec).

### #3.4 — Multi-session working memory (CONFIRMED)
The phase can span ~3 sessions; external-prompt research pauses (3.2.c) are the main session breaks; and it can't lean on `now.md` (which doesn't exist yet — it's derived from the spec being built).
- **Working doc = a single file: `docs/work/consolidation.md`** (name soft, tracks phase/skill name). This IS the project-altitude `brainstorm.md` referenced throughout 3.2/3.3. One file, not a folder — durable outputs live elsewhere (`docs/research/` reports, `docs/spec/` foundation); this is the transient scaffolding that produces them. **Retained after the phase as the genesis record (not auto-deleted).**
- **Structure = brainstorm.md format + a status header:**
  - **"You are here" header** (top): current phase (A/B/C/D), the gap being walked, what's blocking. When research is out: "PENDING: prompts 03/04 handed off <date>; reports expected at `docs/research/03-*.md`,`04-*.md`; resume Phase B gap '<name>' when they return." — the precise resume pointer.
  - **Gap-map as the progress tree** (Phase A gap list, marked as gaps resolve).
  - **Full decision sections** (one per resolved gap, product + technical, path-referencing `docs/research/`).
  - **Open / pending / deferred** (unresolved gaps, deferred two-way doors, pending prompts).
- **Resume mechanic = a bootstrap `now.md`.** At Consolidation start, write a minimal `now.md`: "Project genesis in progress → resume from `docs/work/consolidation.md`." Keeps v2's session-start rule uniform (*always read `now.md` first*) even before the spec exists; the working doc's status header carries the detail. In Phase D / #4, `now.md` is **rewritten** into its real derived form. Not a contradiction of "#4: now.md is derived" — the bootstrap is a transient pointer, replaced wholesale by the derived version. *(Rejected alt: no bootstrap, special-case session-start with "if no `docs/spec/`, look for `consolidation.md`" — cleaner on disk but complicates the single session-start rule.)*
- **`context-capture` keeps it live** — the always-active skill writes decisions into the working doc the moment they surface, so it's never stale at a session boundary. No separate "save state before stopping" step.

### #3.1 — Assessment / gap-map (CONFIRMED)
- **Read all of `intake/` by CONTENT, not filename** (filenames carry no assumptions — `notes.md` might be the whole bible; `spec.md` might be half an idea). Parallel reads ≤4 files, merge-files.js for 5+.
- **Score against a fixed dimension checklist, three tiers:**
  - **Product** — what it is / who for, core features, V1 scope, non-goals, key flows
  - **Technical (weighted heaviest)** — stack (+versions/rationale), architecture, data model, integrations, deployment target
  - **Contextual (optional)** — market validation, constraints (budget/timeline/platform), success metrics. Captured if present, never required.
- Each dimension → **covered / partial / missing / N/A**, with the specific evidence or the specific gap. N/A where a dimension doesn't apply to the project type (a library has no deployment target; a CLI has no data model). Fixed checklist, adaptive marking.
- **Produce a gap-map artifact:** per-dimension status + an **ordered gap list, technical-critical first**. Written down; also **seeds the 3.4 multi-session working doc** (exact path/format settled in 3.4).
- **Present + confirm gate:** agent shows "here's what I think you're building / what's solid / what's missing / the order I'd work it"; user corrects **before** any gap-filling. The agent's read of loose intake can be wrong — validate the map before investing effort.

---

## OPEN branches (the discussion map)

Front-of-lifecycle top-level branches:
- **#1 canonical target** — CLOSED
- **#2 intake** — CLOSED
- **#3 normalize phase** — CLOSED (all sub-branches 3.1–3.4 resolved; sub-map below)
- **#4 derivation** — CLOSED. Phase E of `project-init`; auto-generate CLAUDE.md/now.md/backlog.md from the finished spec → review gate; one confirm only if V1 start is ambiguous; migration needs no special path (old files were intake, already consolidated). Trio held (CLAUDE.md/now.md/backlog.md); the broader "derived set" question → became #7 (its own doc). now.md shape → #8.
- **#5 greenfield vs migration** — CLOSED (single flow)
- **#6 skill topology** — PARKED (its own discussion: one skill or several? how triggered? does the phase reuse the existing `brainstorming` skill or is it its own thing? — note 3.2.a already settled the engine reuses the brainstorming *loop* but is its own project-altitude skill).
- **#7 the KNOWLEDGE LAYER** — **MOVED to its own doc `design-skill-ecosystem.md`** (approved 2026-07-17). Next action there: walk branch #1 (skill anatomy standard).
- **#8 now.md reduced role** — CLOSED (confirmed 2026-07-17). now.md = **thin cursor**: active topic pointer only, written only at topic boundaries (not per-task — write cost), **per-topic/milestone status lives in the topic folder itself** (plan.md checkboxes = source of truth), **next-action inferred** from the topic folder's artifacts at session start (not stored). Value = "what was I doing" cold-start fallback + uniform session-start entry; bypassed when the user names a topic directly. Makes #4 derivation + 3.4 bootstrap now.md *thinner*, consistently. Detailed per-topic status-tracking mechanism deferred to when topic-folder structure is designed (middle-of-lifecycle).

#3 normalize-phase sub-branches:
- **3.1 assessment / gap-map** — CLOSED (above)
- **3.2 technical core** — IN PROGRESS.
  - **3.2.a engine** — CLOSED (reuse brainstorming loop, own project-altitude skill → `docs/spec/`).
  - **3.2.b what "resolved" means** — CLOSED (research not code-spikes; one-way/two-way door depth governor; code-only unknowns → flagged first spike).
  - **3.2.c how `research` plugs in + storage split** — CLOSED (on-demand per-gap; external-prompt = multi-session driver; reports → `docs/research/` referenced not inlined; synthesis+decision → working doc/spec; `<NN>-<slug>.md` naming; not unified — nested per topic later; `research-evaluation` out of scope).
  - **3.2 technical core is now CLOSED** (a+b+c done).
- **3.3 gap-filling interaction + stop condition** — CLOSED. Stop-condition half outcome-defined (above); interaction half = explicit 4-phase walk (Assess → Walk-with-mode-switching → Propose-structure → Write-spec), product interview-led vs technical research-led.
- **3.4 multi-session working memory** — CLOSED (single file `docs/work/consolidation.md` = project-altitude brainstorm.md; status header + gap-map tree + decision sections; bootstrap `now.md` as uniform resume entry; `context-capture` keeps it live).

---

## Pending mechanical fix (not yet done, awaiting go)
- `agentic-workflow_v2/docs/work/roadmap.md` → replace with `backlog.md` (settled from framework-build D16: no roadmap; flat backlog — no categories, no ordering, no status). Not touched yet.

---

## #7 — The knowledge layer → MOVED to `design-skill-ecosystem.md`

> **This section is now a dedicated design doc: `new-workflow/design-skill-ecosystem.md`** (spun out 2026-07-17, user-approved — it spans the whole skill/knowledge/publishing ecosystem, beyond front-of-lifecycle). The corrected model, the `debug-web-pages` study, and the 6-branch A-to-Z map live there. **Next action for #7: walk its branch #1 — skill anatomy standard.** The detail below is retained as historical backup only.

<details>
<summary>Historical #7 notes (superseded by design-skill-ecosystem.md)</summary>

### #7 — The knowledge layer (research + framing)

> **CORRECTED MODEL (from studying `temp/debug-web-pages/` — the user's own reference implementation).** My earlier takes (both "project-specific bucket" and the "project KB → personal KB → skill three-tier pipeline") are SUPERSEDED. The real model:
>
> **The SKILL is the atomic unit of reusable, growing, publishable knowledge.** There is no separate "knowledge-base store" beside skills — a skill's internal `knowledge/` folder IS the cross-project knowledge base. `debug-web-pages` anatomy: `SKILL.md` (stable core — the loop/mode-selection/pointers; the "predictability anchor," changes rarely) + `knowledge/` (its own KB, **churn-separated**: slow shared method files vs `domains/<x>.md` = high-churn, append-only, **isolated** — adding one touches nothing else, zero merge cost) + `tools/` (scripts) + `DESIGN.md`/`ROADMAP.md`/`MAINTAINING.md` (the meta-layer that lets it grow without rotting and get published).
>
> **Anti-rot engine (from `MAINTAINING.md` — this is the answer to "I always miss/lose conventions"):** (1) **churn-separation** — split content by how often it changes (stable core / slow shared / high-churn isolated domain files); (2) **Promotion ritual** — when a tactic appears in ≥2 domain files, lift it to the shared file (single source of truth) *before* it spreads; (3) **Pruning ritual** — periodic relevance + no-op pass, delete aggressively ("sediment is the default outcome without this pass"); (4) **routing rules** — "where new knowledge goes" (one-page fact → domain file; general tactic → shared; new capability → tools + knowledge + SKILL.md mode entry); (5) **hygiene** — verified-only, dated, cite source; (6) **writing style** — prompt the positive, single source of truth, leading words.
>
> **Compounding loop:** "before you start, check `knowledge/domains/` for what we already proved; when you finish, append verified findings." The tooling is fixed; the *expertise* compounds. That's the whole reason it's a skill not a script.
>
> **Publishing (from `ROADMAP.md`):** project-local skill → **hostable personal-skills repo** (modeled on `mattpocock/skills`): `skills/<category>/`, `scripts/link-skills.sh` (symlink-install so `git pull`/edit updates every project live), `.claude-plugin/plugin.json` (installs as one named plugin), `in-progress/` + `deprecated/` categories. **Migration is low-friction by design** = *add* plugin.json + install script + README around an unchanged folder; no move of internals, no path rewrites. Release discipline (changesets, CHANGELOG, ADRs, docs/) added ONLY when publishing to strangers.
>
> **Two destinations, decided by REUSABILITY** (the surviving kernel): reusable domain/stack/method knowledge → a **skill** (grows in `knowledge/`, publishable); project-specific knowledge (this repo's conventions/decisions) → stays in the **project** (`docs/spec/decisions`), never a skill. And: don't *re-document* what a mature skill already covers (external or yours) — reference it.
>
> **The workflow (agentic-setup) and skills are SEPARATE publishable artifacts.** Workflow consumes/recommends skills; skills are independently versioned/published; some are workflow-coupled, some standalone. The user intends to publish BOTH the workflow and their own skills.
>
> **A-to-Z sub-system map (each its own branch — DESIGN NOT STARTED):**
> 1. **Skill anatomy standard** — the debug-web-pages shape as a reusable template; when each meta-doc (DESIGN/ROADMAP/MAINTAINING) is warranted vs overkill.
> 2. **Growth/maintenance framework** — generalize MAINTAINING (churn-separation, promotion, pruning, routing, hygiene, style).
> 3. **Capture/routing loop** — during work: project-specific vs reusable decision → route to project docs vs a skill's `knowledge/`; capture-on-finish; promotion trigger. Rides on `context-capture`. (The part that fixes "I miss things.")
> 4. **Publishing/packaging framework** — the personal hostable skills repo; low-friction migration; release discipline gating; how the workflow references external + personal skills.
> 5. **Init integration (seam to `project-init`)** — at init: read tech spec → recommend/import relevant skills (external + personal) → scaffold project-local capture. Where #7 touches #4/seeding.
> 6. **Skill-creation trigger** — when a recurring pattern graduates from project notes into a new skill; who authors it (existing `write-a-skill`/`writing-skills`?).
>
> **APPROVED 2026-07-17:** split into `design-skill-ecosystem.md`; walk #1 (anatomy) first. Done — content lives in that doc now.

**The gap, precisely.** v2's **skills = process** (how to brainstorm/plan/execute — universal, ship with the template). But every project accumulates **knowledge** that is neither process nor universal, and skills structurally can't hold it. framework-build had a **guide system** for exactly this; v2 kept the process half (as skills) and **dropped the knowledge half**. That dropped half is the user's pain ("I always miss conventions/rules/patterns").

**What framework-build's guide system was** (recovered from `framework-build/docs/guides/` + design-session D35–85):
- **Three tiers** (D61): `docs/guides/core/` (workflow behaviors + universal technical), `docs/guides/domain/` (problem-domain knowledge — e.g. `chrome-extension.md`), `docs/guides/stack/` (library patterns/gotchas — e.g. `zod.md`, `tanstack-query.md`).
- **On-demand, self-selected** (D60): each guide has `name` + `description` frontmatter; agent reads by judgment when the task warrants — never auto-triggered. Discovery via `list-guides.sh` (scans frontmatter → `path | description`). CLAUDE.md lists core guides inline.
- **Body = steps / reference / mix** (`core/guides.md` is the meta-guide: frontmatter rules, body structure, file layout, tier folders).
- **Capture/update model — the part that targets "it gets missed"** (D46/D47/D69, D69 supersedes D47): **never live-edit guides mid-work.** When something worth capturing surfaces → write a note to `docs/work/milestones/<slug>/guide-notes.md`. **At milestone close**, surface the notes → discuss each with user → edit the real guide files for approved items. Agent NEVER auto-updates a guide. Three entry modes: during-work note, manual `/review-guide <path>`, direct user reference.
- The domain guides are **dense, high-value artifacts** (e.g. `chrome-extension.md`: three-world mental model, world-named folder structure, named boundary clients, file-naming table, "adding a new X" checklists, what-to-avoid). This is the hard-won knowledge that skills can't carry.

**Why it was dropped:** D60 made the guide system *replace* the skill system entirely (tool-agnostic goal). When v2 reverted to skills (Claude-specific, richer), the guide system was thrown out with it — **baby with the bathwater**: the knowledge-layer value didn't need to die with the tool-agnostic experiment.

**The reframed problem (what we're actually solving):** a **project-specific, emergent, retrievable, growing** technical-knowledge layer, in ≥3 kinds — **conventions** (this project's naming/structure/idioms), **domain patterns** (how to approach this problem domain), **stack gotchas** (per-library patterns). Properties: mostly emergent (discovered during implementation; some seedable at init from the tech spec), read on-demand (context cost), accumulates over the project's life, and **easily missed** (the crux — emerges mid-work with no disciplined moment to record it).

**Design questions to walk (NOT yet decided):**
- (a) **Structure** — adopt the same 3 tiers (conventions/domain/stack)? Where do they live in v2 (`docs/guides/`? other)? Relationship to the existing `docs/spec/decisions`.
- (b) **Seeding** — does `project-init` derive initial domain/stack guides from the tech spec at init (Phase E-ish), or is it purely emergent?
- (c) **Retrieval** — description self-selection + discovery script (framework-build's model), and how CLAUDE.md points at it.
- (d) **Capture reliability (THE CRUX)** — how emergent knowledge reliably gets recorded. Candidates: framework-build's notes-first + milestone-close review-gate; v2's always-active `context-capture` (but passive alone has missed things per the user); an explicit extraction checkpoint (e.g. at verification / milestone close). Likely a combination.
- (e) **Generalization** — a framework that works for ANY domain, not hardcoded example guides. framework-build's guides were their-product-specific; v2 needs the *system*, seeded per project.
- **First branch to lock:** the PROBLEM DEFINITION + the "knowledge-layer-alongside-skills" framing (user wants to nail the problem before designing the solution).

</details>

## Reference pointers
- `new-workflow/design-skill-ecosystem.md` — the #7 knowledge/skill/publishing ecosystem design (its own doc).
- `framework-build/docs/design-session.md` — D8 (project-level docs: product/tech/decisions), D9 (greenfield design phase), D16 (backlog not roadmap), D23 (three scenarios: greenfield / migration / cycle), D45–46 (brainstorm not implementation-specific).
- v2 skills already built: `agentic-workflow_v2/.claude/skills/` — brainstorming (+write-spec.md), writing-plans, executing-plans, context-capture, research, research-evaluation, visualization.
