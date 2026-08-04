# Design — Brainstorm rework (two modes)

_Started 2026-07-27. Driven by the `read-aloud-app/case3` study case (`tmp/study-cases/read-aloud-app/case3/{brainstorm,spec}.md`)._

The `brainstorm` skill was built for one shape of work — brainstorm a feature, write one spec, plan it, build it. It has exactly one exit, and that exit is sized for one buildable thing. When the brainstorm resolves something bigger, everything above phase-1 altitude falls out of the flow.

---

## The evidence (read-aloud case3)

Brainstorm: 1069 lines, five top-level branches closed — product definition and positioning (A), business strategy and license (E), where synthesis runs across three product phases (B), the whole reading pipeline (C1–C5), caching (D).

Spec: 309 lines, titled "Reading Engine **v1 (web app)**". Good spec — scoped, testable requirements, real success criteria. Covers phase 1 only.

Stranded — resolved in the brainstorm, no home downstream:

- **Branch E, almost entirely.** AGPLv3+CLA survives as one line. Commercial-embedding license as primary revenue, premium/BYO-key voices, donations and corporate sponsors, trademark protection, contributor economics, the AGPL-over-GPL network-clause argument — the spec lists all of it under *out of scope*, "E-branch; not code."
- **A2/A3** — universal positioning, competitive scan, the unoccupied-intersection wedge. Absent.
- **Phases 2 and 3** — browser extension, local daemon, hosted synthesis. B4 locked "all three, chosen at runtime." Spec ships the interfaces, drops the reasoning.
- **The adapter/extractor plugin surface** — a first-class architectural direction for phase 2 plus the business split it enables (core engine = monetizable IP, adapters = community zone). Lives in one paragraph at the bottom of Branch E and nowhere else.

Two fixes rejected before they get proposed again:

- **"It's all in brainstorm.md, nothing is lost."** True in the letter. brainstorm.md is a *process log* — ordered by when decisions were made, layered with corrections. Five places tell the reader an earlier claim was wrong rather than replacing it. Answering "how does this make money" means reading E1, E2, E3 and a separate safety-notes paragraph. It's the transcript, not the reference — the same distinction that separates `/compact` from `handoff`.
- **"Make the spec cover all three phases."** The spec feeds `write-plan`, which feeds `execute`. A spec spanning three phases plus a business model produces a plan nobody can execute. Widening the spec breaks everything downstream.

---

## LOCKED decisions (2026-07-27)

### #1 — Two modes, not three

**Full-product brainstorm** and **topic/feature brainstorm**. These are the two categories faced so far and the two most likely to recur. No third category is assumed; if one shows up it gets designed then.

### #2 — Full-product mode does NOT produce a spec

Its output is a larger, more general, multi-file **product foundation** — working name "product bible." Not a product spec.

Scale evidence (user): read-aloud case3 was **minimal** product brainstorming — a few hours. A labs-scale product took ~two weeks and spanned many areas including heavy marketing work and heavy **UX** design (UX, not UI — for that product type it was load-bearing to whether the product would work at all). The output there was many files, most carrying substantial product detail. Assume the same shape here.

### #3 — That output is `docs/spec/`, the structure already designed in `project-init`

No new canonical target gets invented. `design-project-genesis.md` #1 already locked it: stable base = **product, tech, decisions**; each base file may split into several (tech especially); plus **emergent** file types as the material warrants (open-questions, market-validation, marketing reports…); the agent **proposes the final structure at the end of the phase**, base trio always present.

When the user brainstorms a whole product directly, the brainstorm builds up `docs/spec/` directly.

### #4 — No intake folder on this path

`project-init` assumes the user drops existing material into `docs/intake/`. A from-scratch product brainstorm has nothing to intake — the user has not brainstormed the idea yet, so the material is *generated in-session* rather than synthesized from pasted sources. This path must not depend on `intake/` existing.

### #5 — Topic/feature mode keeps its current shape, with modifications

brainstorm → spec → plan stays. Modifications below (#6 and the smaller issues).

### #6 — Phases must be documented, never stranded in the session

Today, when a large topic resolves into several phases or milestones, `write-spec` silently writes phase/milestone 1 and the rest survives only in the chat session or scattered through brainstorm.md.

The phase cut must be **explicitly written down**: how many phases, what each one is, what's in and out of each. brainstorm.md is an acceptable home — a dedicated file is not required. What is required is that the scope is documented somewhere durable and stated as a deliberate decomposition, not left implicit.

*(Supersedes the earlier `scope.md` proposal from this session — a separate file is optional, not the point.)*

### #7 — Delivery: a sibling sub-file to `write-spec.md`

The full-product path gets its own sub-file in the brainstorm skill folder, beside `write-spec.md` and `write-plan.md`, read when the flow reaches it. Same mechanism as the existing sub-files. Name TBD (verb-first per catalog convention).

### #8 — `project-init`'s scope is changing; out of scope here

Its scope will change significantly based on input the user has yet to give. That is its own discussion. This design must not try to settle it — only to avoid contradicting the parts already locked (#1 canonical target, #3.2.a project-altitude engine reuse).

---

## UPDATE 2026-08-02 — product mode moves to the front of the build queue

From `design-init-flow.md` `## SESSION 2026-08-02 — Flow goes global`. Three changes land here:

- **Product mode is now the entry point to the whole workflow**, not a step after `init-flow`. It runs **anywhere — no repo, no setup, no Flow install** — because commitment to a project is an output of the brainstorm, not a precondition. The user's real pattern: an ideas repo holding many half-formed products across many sessions, most of which never become projects. Nothing in this mode may depend on a project layout existing.
- **It opens with a profile check.** If `~/.claude/CLAUDE.md` lacks the Flow profile, product mode **redirects the user to `setup-flow-globals`** and does not do that work itself (user was explicit: writing the global profile must not be a sub-feature of brainstorming).
- **Branch #D (overlap with `project-init`) is fully closed.** `init-flow` no longer exists as designed; its successor `migrate-to-flow` runs *after* product mode, only for existing codebases. No convergence question remains.

**Watch while writing #C (the engine):** product questions are **elicitation** ("what are you actually trying to do"), technical questions are **propose-and-react**. If those need genuinely different rules, that is a sub-file in this skill. It is *not* a standalone interview skill — Matt's `grilling` is seven lines and Phase 2 already contains all of it.

Build order: this skill is **step 2**, after `flow/` is restructured for the global split and before `setup-flow-globals` and `migrate-to-flow`.

---

## OPEN branches

- **#A — The full-product output's actual file set.** Base trio plus which emergent files? How do marketing and UX/UX-research areas map onto it? Does the agent propose the structure at close the way `project-init` Phase C does, or is there a default set? This is the hard part and needs a real walk — "tricky to get right" (user).
- **#B — Mode selection.** Asked at Phase 1, inferred from the request, or a separate entry point? The two modes have different engines and different outputs, so picking wrong is expensive.
- **#C — Engine for full-product mode.** Same branch-tree walk at a higher altitude (what `design-project-genesis.md` #3.2.a concluded for Consolidation), or something different? Areas like marketing and UX may not decompose into a branch tree the same way technical architecture does.
- **#D — Overlap with `project-init`.** Both paths end at `docs/spec/`. One starts from pasted intake, one from nothing. Are they one skill with two entries (the "single adaptive flow" of #5 in the genesis design), or two skills that converge on the same target? Blocked in part on #8.
- **#E — Does full-product mode end by deriving CLAUDE.md + backlog.md?** `project-init` #4 makes that derivation the closing step. If the direct-brainstorm path produces the same `docs/spec/`, it plausibly inherits the same closer.
- **#F — Sub-file name** for #7.
- **#G — Where the phase cut lives in topic mode** — a `## Phases` section in brainstorm.md, or its own file when large. Low stakes; settle while editing.

---

## Smaller issues in the current skill (unblocked, no open questions)

1. **Delete the resume pointer.** case3's brainstorm.md opens with a 78-line "Session resume pointer" (lines 9–86) — dated status, updates layered on updates, superseded claims corrected inline. That is `handoff`'s job now. The brainstorm skill should stop producing it.
2. **Superseded content gets rewritten, not layered.** Five places in case3 tell the reader an earlier claim was wrong instead of replacing it (C1's "Update 2026-07-22", C2's "the earlier claim was WRONG", C4/T3's "This FALSIFIES", D's "the old wording predates Option B", HeadTTS facts #3's "CORRECTED"). Reader has to reconstruct current truth by diffing. Rule: rewrite the section to current truth; keep one line about what changed only when the reversal is itself informative. Git holds the old text. The current "write everything, never lose a detail" rule reads as "never delete" and produces archaeology.
3. **Research and test scaffolding does not belong in brainstorm.md.** case3 lines 348–374 (research execution plan with prompt paths), 643–655 (a three-test plan), 425–457 (an on-resume instruction block plus a scripted proposal). None of it is a decision. `design-project-genesis.md` #3.2.c already settled the storage split — heavy reports to `docs/research/` referenced by path, synthesis and decision into the working doc — but the brainstorm skill never says it.
4. **The skill contradicts itself on out-of-scope items.** SKILL.md line 120 says out-of-scope items must not land in brainstorm.md, invoke `note` instead. Line 138 says write deferred/out-of-scope items into brainstorm.md under `## Deferred / out of scope`. *Deferred but in scope* and *out of scope entirely* are different things; blurring them means neither rule gets followed.

## Do not touch

The branch tree, the one-branch-at-a-time walk, agent-proposes-first, and the **Decision / Reasoning / Rejected** structure of the decision sections. Those are the best part of the case3 artifact — C5a–C5e, D, B and E are excellent records. This is a seam problem, not a demolition.

---

## SESSION 2026-08-04 — full restart; the two-mode premise is void

Walked conversationally. **Every structural proposal made this session was rejected by the user.** What survives is evidence, constraints, and a list of dead ends — recorded so the restart does not re-walk them. The restart designs from scratch.

### The two-mode premise is dead

User, unprompted: *"I really don't like having modes… there will be a lot of stuff that will be kind of maybe in between. I felt like having just one brainstorming skill with a single mode that could handle everything would be better."*

Voids **#1** (two modes) and, with it, **#2**, **#5** and **#7** — each exists only to describe the second mode. **#3** (target is `docs/spec/`), **#4** (no `intake/` dependency), **#6** (the phase cut must be written down) and **#8** survive; none needs modes. Open branches **#A**, **#B**, **#C**, **#E**, **#F**, **#G** are void as phrased — every one presupposes a full-product *mode*.

Scope widened too: the user's judgement is that `brainstorm`, `write-spec.md` and `write-plan.md` all need rewriting rather than patching, and `execute` is implicated through its dependence on one large plan file. This does **not** overturn `design-init-flow.md` #6b — the walk itself (tree, one branch at a time, agent-proposes-first, Decision/Reasoning/Rejected) is still the part that works.

### Study case — `tmp/local-refs/delapse-docs`

The user's own SaaS project, ~40 milestones deep, run on the v1 workflow. Full `docs/` tree, copied in by the user 2026-08-04 specifically as a test case: *"that was a terrible flow we went with — I just want to make sure this new flow can really tackle such tasks."*

| | |
|---|---|
| size | 180 files; `work/` alone is 80,229 lines |
| `plan.md` | the bulk of it — m10 2,977; m07 2,875; m11 2,664 |
| `spec.md` | ~30 files, 10,264 lines, ~340 each |
| `issues.md` | 22 files, **608 lines total** (~28 each) |
| `roadmap.md` / `now.md` | 385 / 417 |

**The system diagnosed itself.** `work/audit/index.md`, written by that project's own agent: *"The milestone specs captured only each milestone's initial phase, not the full feature scope, and designs changed underneath them. So `now.md`, `roadmap.md`, and the milestone specs systematically under-count what's missing. This folder is the trustworthy inventory we carve milestones out of — code-truth first, not doc-truth."* After ~40 milestones the project had to re-derive nine documents from the code because every planning document had drifted out of trust.

**Small append-as-you-go files stayed true; large forward-looking files rotted.** `roadmap.md` carries strikethrough entries and one that says outright "the list to work from is `docs/work/audit/bugs.md`, not this entry." `now.md` was specified as a thin cursor and reached 417 lines. `issues.md` — ~28 lines per milestone, written *during* the build — is the highest value-per-line artifact in the set.

**Spec and plan said the same thing twice.** `m00-monorepo-scaffold/spec.md` (422 lines) contains `turbo.json` verbatim, `.npmrc` contents and every pinned version; `plan.md` (1,331 lines) then repeats all of it as steps.

**The milestone kept splitting under pressure** — m07c, m07d, m08e, m12a/b/c, m15a, m16a, m17a, m19a, m24a/b, m29a/b/c. Plans died before shipping: `plan-superseded.md`, `plan-b.md`, `plan-draft.md`.

**Implementation surprises were caught by reading code, not by writing code into plans.** `m29a/issues.md`'s findings — no repository has a delete method, pg-boss cancellation unusable per-entity, `LOG_LEVEL` never did anything, `AI_MOCK`/`TEST_REAL_LLM` were one decision spelled twice — sit under the heading **"Pre-planning verification."** The 2,227-line plan did not find them; a deliberate pass against the code did.

**`docs/spec/` is not accused.** The audit names `now.md`, `roadmap.md` and the *milestone* specs. The project-level folder (`PRODUCT-SPEC.md` 695, `DECISIONS-LOG.md` 560, `product-brief.md` 210, `tech.md` 182, `OPEN-QUESTIONS.md` 102) is not on that list.

**Not everything lived in a milestone.** `work/` also holds `audit/`, `voiceover-feature/`, `flow-brainstorm/`, `llm-mock-brainstorm/`, `dev-panel-program.md`, `financial-plan.md`.

### Constraints the restart must satisfy — all user-stated this session

- **The tree stays.** Free-form, arbitrary depth, sub-branches added mid-walk, jumping back up to a root branch. Its freedom is the point.
- **One flow, no modes.** In-between cases are the norm, not the exception.
- **A shipped milestone's spec is a history log.** It records what was executed; going stale afterwards costs nothing — *"that old milestone can stay as is, and we're obviously going to come up with a new milestone to rewrite it."* Do not design for its survival.
- **The milestone earned its place by being testable.** It ends where a large chunk can actually be tried. Some work cannot be exercised at all until several milestones complete — the user's example: a free-trial abuse-prevention pipeline spanning three or four. **Any smaller unit must not push the first moment of real testing further out.**
- **Complete code in plans existed for a reason** — to surface implementation surprises at plan time, when they are cheap. Any replacement must say concretely how those surprises get caught instead.
- **Change is the normal case.** A U-turn after milestone 1 is expected, decisions are not locked, and problems are often visible only after implementing. The user: solo dev, somewhat experienced in SaaS, not experienced at this scale, works "in kind of chaotic flow." A `prototype` skill is planned to reduce this, but the workflow must handle it either way.
- **Backlog and roadmap are to be deprecated**, at least partly, in favour of specs and tickets.
- **Wayfinder is reference only.** The user has not read it; anything taken from it must be explained from zero, and dropping it entirely is explicitly acceptable.

### Rejected this session — do not re-propose

- **A flat index file** (`map.md` with Open / Decided / Fog sections) replacing the tree. Loses the free-form depth that makes the tree work.
- **One file per decision, one file per task.** No principled rule for where the splits fall, and it scales to hundreds of files. The 2,000–3,000-line plans have a simpler cause: the rule requiring complete code in every step.
- **"Would this sentence still be true if it were built a different way? Yes → spec, no → tasks."** Applies a rule meant for a re-read document to one that is a record. User: *"That doesn't really matter, because that's just a history log."*
- **Adopting delapse's `bugs.md` / `debt.md` / `checks.md` as workflow files.** The sharpest rejection of the session, and correct: `checks.md` is browser QA for a Chrome extension, specific to that project, and the whole `audit/` folder is a **recovery artifact from a broken process**, not a designed workflow. Reusing it imports the v1 flow this rework exists to replace. General lesson: the study case is evidence of what *failed*, never a file set to copy.
- **Renaming `plan.md` → `tasks.md`/`tickets.md`, `issues.md` → `notes.md`.** Churn with no argument behind it.
- **"Ticket" as vocabulary.** User: *"to me it seems like ticket is more like a single task rather than ticket."* It is the same unit already called a task.

### Method note for the restart

The session failed by moving the proposal every turn in answer to each objection, so there was never a stable target to react to, and each answer reintroduced a problem an earlier one had solved. Fix a shape first, then work objections against it without redrawing it mid-argument.

---

## Reference pointers

- `tmp/local-refs/delapse-docs/` — the 2026-08-04 study case (180 files; `work/audit/index.md` is the self-diagnosis).
- `tmp/study-cases/read-aloud-app/case3/` — the study case (`brainstorm.md` 1069 lines, `spec.md` 309).
- `new-workflow/design-project-genesis.md` — #1 canonical `docs/spec/` target, #2 intake, #3.2.a project-altitude engine, #3.2.c research storage split, #4 derivation, #5 single adaptive flow.
- `flow-skills/skills/brainstorm/` — `SKILL.md`, `write-spec.md`, `write-plan.md`.
- `new-workflow/design-explain-rework.md` — the parallel explain/CLAUDE.md thread.
