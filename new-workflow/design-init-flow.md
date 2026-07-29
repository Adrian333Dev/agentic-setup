# Design — `init-flow` (front door) + the project doc set

_Started 2026-07-29. Supersedes the front-half of `design-project-genesis.md`. Continues the deliberation-cost thread from `design-explain-rework.md` and `design-capture-rework.md`._

`project-init` was one name over two jobs. This thread splits them, names the front door, and settles what documentation a Flow project actually carries.

---

## LOCKED decisions

### #1 — `project-init` splits; the deliberative half leaves

`project-init`'s five phases (Assess intake → Walk gaps → Propose spec structure → Write spec → Derive scaffold) are two different things wearing one name:

- **A–D** is a multi-session, research-driven branch walk that produces the whole product foundation. That is `brainstorm`'s full-product mode, already under design in `design-brainstorm-rework.md`. It moves there.
- **What's left** is the front door: land Flow in this repo, fill what can be filled, wire skills, point at the next move.

Same cut as the last three threads — cheap/mechanical vs expensive/deliberative. Assessing pasted material is a judgment call, so it goes with the deliberative half; product mode opens by asking whether there's material to assess, which is the "single adaptive flow" of genesis #5 relocated to the right skill.

Kills `design-brainstorm-rework.md` #D (overlap with project-init) and genesis #6 (skill topology).

### #1b — `docs/work/consolidation.md` is dissolved

It existed only because `project-init` was a separate skill needing its own working memory. Genesis #3.4 says outright that it "IS the project-altitude `brainstorm.md`" — one artifact under two names, which is why the user couldn't say what it was for.

Product-mode brainstorm writes `brainstorm.md` at project altitude (`docs/work/brainstorm.md`, not under `topics/` — it isn't a topic). Its "you are here" status header is separately dead: `handoff` owns resume state since `now.md` was killed, and the brainstorm rework already lists "delete the resume pointer" as a fix.

The user's second concern — one file will not hold a product brainstorm — is real and survives; see Parked (wayfinder index-vs-store) and `design-brainstorm-rework.md` #A.

### #2 — Name: `init-flow`

Verb-first per catalog convention. Avoids Claude Code's built-in `/init`. Names the actual job — initialize Flow in this repo, not initialize a project. It never decides what the product is.

Re-runnable: the same skill updates an existing Flow project to a newer template version (see #7).

### #3 — `flow/CLAUDE.md` stays ONE file — split REJECTED

Proposed and rejected same session. The proposal was to move the workflow half (`## Explaining`, `## Communication`, `## Capture`, `## Workflow`, `## Scripts`, `## Hard rules`) into an `@`-imported file, leaving the project half in CLAUDE.md.

Three reasons it fails:

- **It violates the principle the last three threads established.** Never put an always-needed rule behind a mechanism that can fail to fire. An `@` import is exactly that, and it fails *silently*.
- **It doesn't solve migration.** The migration hazard is contradiction, not volume. Two files that contradict each other are worse than one file that does — in one file the conflict is visible and adjacent.
- **The two-lifecycles framing was wrong** (user). The project half starts as placeholders and fills over the whole life of the project via `## Capture`. That's one file with one lifecycle, not two glued together.

The update-path argument was real but small, and is solved by making `init-flow` re-runnable (#7), not by file layout.

### #4 — Migration is CONVERSION, not merge

Corrected by the user. Flow **replaces** the user's existing workflow; it does not negotiate with it. Adopting an opinionated workflow means accepting its opinions. Their `ls` rule loses to `tree.sh`, outright.

What migration does:

- **Harvest content, discard process.** Their instruction file is *source material*. Facts about the project (stack, structure, rules about the code, verified commands, hard-won gotchas) get routed into Flow's structure. Their process instructions — session-start rituals, their own doc-reading order, their skill overrides — are dropped; Flow supplies its own.
- **Most harvested material does NOT land in CLAUDE.md.** It lands in `docs/spec/` or `docs/context/`.
- **Their existing docs are intake.** Inventory them, record where they are. `init-flow` never consolidates them — that's a product-brainstorm run, offered and declinable.
- **`.claude/settings.json` merges, never replaces** — they may have their own hooks, permissions, MCP config. Add Flow's keys, keep theirs, report what changed.

### #5 — Blocking git mutations is a Flow default, not a personal preference

Corrected by the user. An agent that can commit and push autonomously has an unbounded blast radius. Flow ships the deny list. Anyone who wants it gone removes it.

### #6 — Borrowing from `reference/mattpocock-skills` requires re-derivation

Standing rule for this thread. His skills assume an issue tracker, GitHub, PRs, and a team-shaped review loop. Flow assumes files on disk and one author. Anything of his that routes through the tracker does not transfer as-is — only the underlying idea transfers, and it has to be rebuilt against files. Never adopt a rule of his together with the environment assumption that made it work.

### #6b — `brainstorm`'s core is not up for rewrite

User: the skill "was decent, it wasn't bad, it worked well — results were always satisfying." Reconsider before rewriting anything there. Consistent with `design-brainstorm-rework.md`'s own "Do not touch" list (branch tree, one-branch-at-a-time, agent-proposes-first, Decision/Reasoning/Rejected sections). The failure that opened that thread was at the **seam** — 1069-line brainstorm producing a 309-line spec that dropped the business model and phases 2–3 — not in the walking.

### #7 — The template needs a changelog

Once published, `flow/` carries `CHANGELOG.md` — template-level, not CLAUDE.md-level. Every change to the workflow gets an entry.

This is what makes `init-flow`'s re-run cheap: instead of diffing two CLAUDE.mds, it reads changelog entries forward from the version the project records and applies only those. Requires the project to stamp which template version it's on (one line; location TBD).

### #8 — `prototype` — PARKED

Investigated `reference/mattpocock-skills/skills/engineering/prototype/`. It answers two questions with throwaway code: "does this state model feel right" (tiny interactive terminal app) and "what should this look like" (several UI variants on one route, switchable by URL param).

Parked because the user's actual need is different: **technical feasibility spikes** — stand up a local model, wire the tools, find out if the thing is possible at all (read-aloud case). That is not what `prototype` does, and Flow already has a designed answer (genesis #3.2.b: code-only unknowns become a flagged first spike in the backlog). Whether that deferral is right is a `brainstorm`-thread question.

Also noted: `explain` partly covers the UI branch already, though **its mockup output is minimal and not good** (user) — separate improvement, recorded in Parked below.

---

## OPEN

### #A — The project doc set — PROPOSAL ON THE TABLE

Sorting the two reference projects into Flow's shape (evidence below) leaves exactly one gap: **project-specific durable knowledge that is neither a rule, nor a decision, nor a skill.** `commands.md`, `setup-notes.md`, `lessons-learned/`, `caching-improvements.md`, the dated debugging writeups. delapse's `setup-notes.md` names the category itself: *"a reference, not a directive — things that were done once, not rules to follow on every file."*

**Proposal: `docs/context/`.** One folder, one file per subject, created on demand. `commands.md` from day one; everything else as the project accumulates it. Four rules, aimed at the observed failure mode (bloat, not absence):

1. **Every file answers one question: what would a fresh session get wrong without this?** That's the entry test, and the one `commands.md` failed in delapse (98 lines, user's verdict: bloated).
2. **Facts, not process.** Process is a skill. A context file describing *how to work* is content in the wrong repo.
3. **Verified only.** delapse's `commands.md` carries `# unverified` on six entries. An unverified command is worse than none — the agent runs it, it fails, and the read plus the failure are both wasted.
4. **Rewrite on change, never append.** Git holds the old text.

Routing already exists: `## Capture`'s inbox catches this class ("reusable knowledge needing an altitude call"), and `organize` files it. Today `organize`'s only destinations are a catalog skill or a `needs skill:` flag — `docs/context/` is the third destination it has been missing, for knowledge that is project-specific rather than cross-project.

**Knock-on: this unblocks the key-docs table** stranded in `design-explain-rework.md`. It should list only *stable* paths — `docs/spec/`, `docs/work/backlog.md`, `docs/context/`, `docs/research/` — plus one line on what lives in `context/`. Never per-file: a table needing an update whenever a context file appears will lie within a month.

### #A2 — Skill vs project-context: the routing test — PROPOSAL

The hard part, and the user's own words: *"some of the content is quite tricky… you're really not sure, is it going to go to the project context files or to a skill; there is stuff that's in between as well."*

**The test: would this sentence be true in a different project?**

- **Yes → skill.** `model-notes.md` (267 lines on Vertex thinking-mode latency, Flash-Lite token profiles, provider rate limits) is the clean example — it reads as project docs but is really portable knowledge about building LLM pipelines. User's correction, and it was wrong to group it with `commands.md`.
- **No → `docs/context/`.** "Verification order is check-types → lint → vitest → build." "Supabase types generate to `packages/contracts/src/supabase/database.types.ts`."

**In-between content splits; it doesn't get assigned.** `never-edit-database-types-manually.md` is both: the principle (generated files are never hand-edited — regenerate) is portable and belongs in a Supabase skill; the specifics (this repo's script name and output path) are local and belong in context. Route each half.

**When you genuinely can't tell, don't force it — that's what the inbox is for.** It stays raw until `organize` / `curate-skills` has enough instances to see the pattern.

**`setup-notes.md` is the pipeline, not a destination** (user). It existed because v1 had no skills. In Flow, that content flows: inbox or project files → `curate-skills` → an actual skill. The file disappears; the flow through it is the point.

### #B — Status/query mechanism — PROPOSAL

Frontmatter on topic files plus a script, not a hand-maintained table. lumacraft's `now.md` had a good 12-row milestone status table — and `now.md` is exactly the file killed for being a maintenance tax. Derived state can't drift:

```yaml
---
status: active        # active | done | blocked | parked
title: Brief agent direct-edit model
blocked-by: gate-a-approval
---
```

`scripts/status.sh` sweeps `docs/work/topics/*/` and prints the table — one cheap call instead of N reads, the same trade `tree.sh` and `merge-files.js` already make. Build item, not a design question; settle once the topic folder structure is final.

### #C — `init-flow`'s job list — NEXT BRANCH TO WALK

What it does on a fresh template versus an existing repo, and whether those are one flow or a fork.

Known inputs to the walk: the template already ships `CLAUDE.md`, `scripts/`, `docs/work/backlog.md` and `.claude/settings.json` via *Use this template*, so there is nothing to scaffold on the greenfield path — the job there is filling `## Project` and `## The user`, and it is small. The migration path is where the work is: read the repo, infer stack and structure, harvest content per #4, inventory existing docs, merge `settings.json`, replace their process. Open question inside this: the skill-recommendation seam (`design-skill-ecosystem.md` #5) reads the tech spec to recommend skills — but on a greenfield path the stack is an *output* of the product brainstorm, not an input, so that seam may only be able to fire on the migration path and at product-brainstorm close.

### #D — Where the template version stamp lives (#7)

### Closed while surveying

- **Script documentation in `CLAUDE.md` is already complete.** Checked `## Scripts` against both script headers: `--depth`, `--except` (repeatable; name/folder/glob), the default ignore list, `--ext`, `--force`, the `file.md:45-89` range syntax, folder recursion, fenced-per-file output, and the 2000-line cutoff are all there. An agent can run both without opening them. Only `check-skills.sh` is unmentioned, which is correct — it's a SessionStart hook, never invoked by hand.

---

## Evidence — two real projects (2026-07-29 survey)

`tmp/local-refs/delapse/docs` (36 files) and `tmp/local-refs/lumacraft_v2/docs` (41 files). Both v1-era, slightly different workflows. Sorted by *kind of content*:

| Kind | Files seen | Where it goes in Flow |
|---|---|---|
| Process instructions to the agent | `workflow-rules.md` (239), `planning.md`, `milestones.md`, `research.md`, `testing.md`, `superpowers-overrides.md`, `prompt-engineering-process.md` | **Deleted.** This category *is* the skills. It only existed because v1 had no skills. |
| Generic stack conventions | `conventions.md` (280 in delapse, 59 in lumacraft) | Mostly **catalog skills** (stack knowledge). The project-specific residue → `## Project rules`. |
| Rules genuinely about this codebase | delapse `CLAUDE.md` `## Project-specific rules` (15 bullets) | `## Project rules` — already correct. |
| Operating facts | `commands.md` (98 / 36), verification order | **Gap.** |
| Learned project knowledge | `model-notes.md` (267), `setup-notes.md`, `lessons-learned/` (8 files), `notes/caching-improvements.md` (375), `debugging/<date>-*.md` | **Gap.** |
| Foundation | `spec/` (7 files / 12 files, 3400 lines) | `docs/spec/` — designed. |
| Working state | `now.md`, `roadmap.md`, `backlog.md`, topic folders | Designed (`now.md` and `roadmap.md` both killed). |
| Vendored external docs | `references/`, `llms/` (llms-full.md dumps) | `tmp/` — refetchable. Non-refetchable prior-project notes → learned knowledge. |

**Two observations that matter more than the inventory:**

1. **Flow deletes the single biggest category.** Roughly a third of both projects' `docs/agents/` is process prose that skills replace outright.
2. **The failure mode is bloat, not absence.** 280-line conventions, 560-line decisions log, 375-line caching notes, and the user's own verdict on `commands.md`: *"kind of bloated, included too much unnecessary shit."* Nothing prunes. So the design problem is not which files exist — it's what keeps them small.

Also worth keeping: `lessons-learned/` entries follow a fixed shape — Symptom / Root cause / Fix / Prevention — and the Prevention section names where the rule should end up (rule, plan, review). That is `organize`'s altitude call, done by hand, and it worked.

---

## Parked — belongs to the brainstorm thread

**Wayfinder ideas worth re-deriving against files (product mode only).** (a) *Index, not store* — the always-loaded artifact holds one line + link per decision; the detail lives in exactly one place. (b) *Fog of war* — an explicit home for questions you can sense but can't yet phrase; the test is whether you can *state* the question now, not answer it. (c) *One question per session.* Topic mode keeps its single `brainstorm.md`; applying this to a two-hour feature brainstorm is strictly worse. Evidence for (a) is case3: 1069 lines for a *minimal* product effort, and the user's report that a labs-scale product brainstorm ran two weeks across many files.

Also parked here: whether a feasibility question should stop the brainstorm and get proven with throwaway code, or stay deferred to a backlog spike as genesis #3.2.b decided (see #8).

---

## Everything else → `new-workflow/backlog.md`

Loose items raised in session and not owned by this thread live there — settings work, skills to build, the faulty subagent-reading principles, hooks, migration of the user's own projects, and the two proposed `CLAUDE.md` rules (cost efficiency, push back before agreeing) awaiting a go.

---

## Reference pointers

- `new-workflow/design-project-genesis.md` — the superseded front-of-lifecycle design; phases A–D migrate to the brainstorm thread.
- `new-workflow/design-brainstorm-rework.md` — full-product mode, where Consolidation lands. #D now closed by #1 above.
- `reference/mattpocock-skills/skills/engineering/` — `setup-matt-pocock-skills` (explore → present → confirm → write, edit in place), `ask-matt` (router), `wayfinder` (map + decision tickets), `prototype`.
- `tmp/local-refs/delapse/docs`, `tmp/local-refs/lumacraft_v2/docs` — the survey above.
