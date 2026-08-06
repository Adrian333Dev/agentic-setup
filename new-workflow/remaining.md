# Remaining — the whole build in one list

Master checklist for Flow v1. Ordered by **what blocks what**, not by size. Every item names where its
design already lives; **no pointer means the design does not exist yet** and deciding it is part of the item.

Absorbs `new-workflow/backlog.md` in full — see the last section.

Legend: ⛔ blocks the items under it · ❓ needs a decision before it can be built · 🔁 rewrite of a file that exists

---

## Where it stands

The design is close to done. **Almost nothing is built.**

`flow/skills/` holds eight skills written against the **old** chain — `brainstorm.md → spec.md → plan.md`,
topic folders at `docs/work/topics/t<NN>-<slug>/`, milestones. The 2026-08-05 and 2026-08-06 sessions
replaced that chain with **topics → one flat ticket pool → a `## Plan` section inside the ticket**, and
added product mode on top. So the skills on disk describe a system that no longer exists. Four of the eight
(`brainstorm`, `execute`, `organize`, `handoff`) name paths and files that the design has deleted.

Nothing is symlinked into `~/.claude/` and nothing is installed — deliberate, until the skill set is final.

---

## Step 2 — the brainstorm → ticket → plan chain ← **current step**

### 2a — Commands: the registration mechanism ⛔

Two separable problems, both open. This one is **how any command gets to be a command** — it outlives the
ticket system and covers tools that have nothing to do with Flow.

Today there are no commands, only paths: `bash ~/.claude/scripts/tree.sh`. The user wants real globally
registered commands, callable by name from any directory, by both the agent and himself.

**The split he stated (2026-08-06):** general-purpose tools (`tree.sh`, `merge-files.js`, a future
git-in-one-shot command) are *not* Flow-specific and should not be namespaced under Flow. Ticket and topic
commands are Flow-specific and only mean anything inside a Flow project.

- [ ] ❓ The PATH mechanism — one directory (`~/.local/bin` or `~/.claude/bin`) added once by
      `setup-flow-globals`, with every command symlinked in. Aliases are rejected: bash does not expand
      them in non-interactive shells, so the agent could not call them
- [x] **Names settled 2026-08-06: `ptree` and `merge`.** `tree` collided with GNU `tree`; `ptree` =
      "project tree". `map` was the rejected alternative — too generic a word to claim globally
- [x] **Language: JavaScript** (node), settled 2026-08-06. The user leaned that way and the work agrees —
      every command is *parse YAML frontmatter across N files → filter → write it back*, plus a dependency
      graph for `next` (transitive dep checks) and `check` (cycle detection). That is bash's worst domain:
      hand-rolled sed/awk against a structured format, and `deps: [t045, t046]` arrays make it worse. Node
      is already a toolchain dependency via `merge-files.js`
- [ ] **Zero dependencies preferred, not mandated** (user, 2026-08-06). Dependencies are allowed where
      genuinely necessary. Test for "necessary": the alternative is re-implementing a spec somebody else
      already got right — full YAML, semver, timezone-aware dates — not merely that it saves typing.
      The real cost is concrete rather than ideological: a dependency means a `package.json` and an install
      step, `setup-flow-globals` grows that step, and the never-run-install hard rule means **the user** has
      to run it before his commands work
- [ ] **Frontmatter: hand-parse it.** Unaffected by the above — seven controlled fields (`id`, `title`,
      `status`, `type`, `topic`, `deps`, `by`) is roughly 50 lines, well under the bar
- [ ] Rewrite every `~/.claude/scripts/…` path reference in `global/CLAUDE.md` and in the skills once the
      commands have names
- [ ] Behaviour when a command is missing — files stay plain markdown + YAML and grep still works, so say
      what a skill does rather than letting it improvise

**Project-root discovery — required (user, 2026-08-06).** Commands take no path. Run from anywhere inside a
project, they locate it from the current directory.

- [ ] **Resolve the root with `git rev-parse --show-toplevel`**, not a hand-rolled walk up the tree. One
      subprocess, and it already handles the cases a walk gets wrong — worktrees (where `.git` is a *file*),
      submodules, and symlinked paths. Nested repos resolve to the nearest enclosing repo, which is the
      correct answer in this workbench (running inside `flow/` must mean `flow/`, not `agentic-setup/`)
- [ ] `.git` is the marker, **not** `docs/tickets/` — that folder does not exist before the first ticket, so
      `flow ticket new` in a fresh project would have nothing to find
- [ ] Not in a git repo → one clear error. Brainstorming works with no repo, but tickets do not exist until
      a project does
- [ ] ❓ `FLOW_PROJECT` env override, for an agent dispatched with a different working directory
- [ ] **Future: one git command** for add + commit + push with flexible arguments. Likely a **git alias**
      (`git save`) rather than a PATH script — idiomatic, zero PATH entries, takes args via `!f(){…};f`.
      **User-run only; the agent never calls it** (user, 2026-08-06). Not part of the Flow command set —
      it is a personal tool that happens to live in the same place
- [ ] ❓ Optional, user's call: add the git command's name to `guard.py`'s deny list anyway. The user's
      position is that it is his command, so the guard is unrelated. Noted counterpoint — the guard does not
      gate *who* runs a command, it gates what the **agent's Bash tool** executes, and the agent can type any
      string. Cost of closing it is one deny-list line; risk of leaving it open is low in practice

### 2a2 — Commands: the ticket/topic surface ⛔ ❓ **not approved**

**Correction on the record (2026-08-06):** the 08-05 command list was never really approved — the user
waved it through to keep moving and considers it "very terribly designed and very confusing." Treat
`design-brainstorm-rework.md` → *Commands, not an index file* as a **first draft**, not a settled surface.

What is concretely wrong with the draft, so the redesign has a target:

1. **`flow ticket` and `flow tickets` are different commands.** Singular/plural as a semantic distinction is
   unmemorable — nobody recalls whether listing is `ticket list` or `tickets`
2. **`flow ticket set t047 status=done`** — key=value is un-guessable next to the obvious `flow ticket done t047`
3. **"status" means two different things** in one CLI: `flow status` (project state) and `--status` (a
   frontmatter filter)
4. **`--ready` is a flag**, buried on a list command — yet it is *the* question the whole no-index-file
   decision depends on. The most-run query should be the shortest thing to type
5. **No topic commands exist at all**, though topics get created, get frontmatter and get `parked`

**SURFACE — CONFIRMED 2026-08-06.** Three tiers: the daily loop is top-level and noun-free because it is
90% of use; everything less frequent is namespaced; overviews stand alone. Both judgment calls below were
accepted as proposed.

```
# the daily loop
flow next                            workable now — todo with every dep done
flow start  <id>                     → in-progress
flow review <id>                     → review
flow done   <id>                     → done

# looking around
flow ls [todo|in-progress|review|done|dropped] [--type …] [--topic …]
flow show <id|slug>
flow status                          active topic, in flight, in review, ready count
flow check                           cycles, dangling ids, dropped blockers

# less frequent ticket edits
flow ticket new "<title>" [--type feature] [--topic <slug>] [--deps t045,t046]
flow ticket drop <id>                warns and lists dependents — a dropped dep must error on them
flow ticket supersede <id> --by t020,t021
flow ticket dep <id> [--on|--off] t045
flow ticket edit <id> [--title …] [--type …] [--topic …]

# topics
flow topic new "<title>" [--from t014]
flow topic ls | park <slug> | commit <slug> | drop <slug>
```

How it answers each fault: singular/plural collision gone — `ticket` only, `ls` is the list verb ·
key=value gone — verbs and flags · **`--status` no longer exists**, so "status" means the project overview
and nothing else; filtering takes a bare positional (`flow ls done`) · `--ready` promoted to `flow next` ·
topics have commands.

Two judgment calls to confirm or overturn: **the daily verbs are top-level** (`flow start t047`, not
`flow ticket start t047`) — ids and slugs are visibly different so it stays unambiguous; and **`flow ls`
defaults to tickets**, with `flow topic ls` for topics — an asymmetry paid so the common case is short.

- [x] Surface confirmed 2026-08-06
- [x] **`design-init-flow.md` #B is superseded — folded in, 2026-08-06.** It was a 2026-07-29 sketch
      (frontmatter on topic files + a `scripts/status.sh` sweeping `docs/work/topics/*/` to print a status
      table) written before topics and tickets were redesigned. It is an earlier, smaller draft of
      `flow status` / `flow ls`, and nothing in it survives that those do not already cover. Mark it
      superseded in that doc during the sweep
- [x] **Templates live beside the script as real files** (user, 2026-08-06) — `templates/ticket.md`,
      `templates/topic.md`, read at runtime. **Not embedded strings in the JS:** a template is content the
      user will want to edit (add a section to every new ticket), and editing a string literal to do that is
      hostile
- [ ] Proposed layout, since JS + templates means a folder rather than a loose file:
      ```
      flow/global/scripts/flow/
        flow.js          entry point + dispatch  (#!/usr/bin/env node, +x)
        lib/             frontmatter, root resolution, the ticket/topic model
        templates/       ticket.md, topic.md
      ```
      Knock-on: `flow/CLAUDE.md`'s layout table lists `global/scripts/` as four loose files
- [ ] Sweep the command names into every doc that references them — **after** the surface is confirmed, once,
      not twice. Same paragraphs also carry the now-overturned flat-file pool wording

**Mandatory use — confirmed by the user 2026-08-06, with the boundary stated.** Creating a ticket or a
topic, and changing status or any other frontmatter field, **must** go through a command. Everything else —
writing the body, the `## Plan`, adding files alongside — is free-hand. The line:

> **Frontmatter is owned by the commands. Everything else is written by hand.**

- [ ] Every frontmatter mutation needs a command, or the agent will hand-edit the uncovered case and the
      invariant dies quietly: create, status, deps, topic, supersede-with-`by`, retitle
- [ ] Write the rule into the skills that mutate tickets, and into `global/CLAUDE.md`

### 2b — Scripts: build

- [ ] `flow ticket new "<title>" --topic <slug> [--deps t045]` — next id, writes from template
- [ ] `flow ticket set <id> status=… | deps=…` — edits frontmatter, validates references
- [ ] `flow tickets [--status] [--topic] [--ready] [--tree]` — `--ready` = `todo` + every dep `done`
- [ ] Integrity reporting inside `tickets`: dependency cycles, dangling ids, `dropped` blockers (a `dropped`
      dep must raise an error on its dependents — that is the only reason `dropped` and `superseded` both exist)
- [ ] `flow status` — active topic, in-progress, review pile, ready set
- [ ] No delete command. `status: dropped` is the archive

### 2c — 🔁 `brainstorm/SKILL.md`

The engine survives ("brainstorm's core is not up for rewrite — the failure was at the seam"), but every
path, the tree notation and the whole close phase change.

- [ ] Zero-based branch indices (`0`, `1`, `2`; children `0.0`, `0.1`) replacing `A`/`B`/`C1`
- [ ] Output split: `tree.md` always whole in one file + `<index>-<name>.md` detail **only for branches that
      actually grew** — not one file per root branch
- [ ] Topic paths: `docs/topics/<slug>/` = `topic.md` + `brainstorm/`, nothing else
- [ ] Topic frontmatter (`slug`/`title`/`status`/`from:` array) and its four statuses
- [ ] Product mode: entered **explicitly**, never auto-detected; brainstorm lives at `docs/brainstorm/`
- [ ] Kickoff: propose root branches **plus walk order**, confirm before walking. Walk order is a recorded
      dependency rule ("branches that constrain other branches go first"), never a fixed template
- [ ] The topic-vs-product discriminator: if every root branch can be resolved in one session, it is a topic
- [ ] Two exits — **commit** (write tickets) or **park** (write none, deliberately)
- [ ] Delete Phase 4's `spec.md` writing
- [ ] Profile-existence check → redirect to `setup-flow-globals`, never do that work inline
      (`design-init-flow.md` #G7)
- [ ] Coordination table: `research`, `explain`, and `prototype` once it exists
- [ ] Where research reports land — `docs/research/`, flat and **global**, not per-topic

### 2d — 🔁 the product-mode conversion sub-file

Replaces `write-spec.md`. It is about **the conversion**, not about brainstorming.

- [ ] ❓ Name it — `write-spec.md` still fits since it writes `docs/spec/`, but it also mints tickets. One
      sub-file or two
- [ ] Write `docs/spec/` in one go from the tree — markdown only
- [ ] `product.md` — the Bible: whole product, every behavior, all versions, plus the **scope ladder**
      (V1 / next / later / never)
- [ ] `tech.md` — stack, repo layout, high-level components (backend / frontend / services / workers /
      packages), high-level design, the decisions that constrain implementation
- [ ] The disjointness rule as the birth rule for any third file: *no fact in two files; boundary statable in
      one sentence; otherwise it is a section*
- [ ] Explicitly forbidden: `decisions.md`, `open-questions.md`, a `README.md` index, frontmatter, copied
      artifacts, history or rationale in spec files
- [ ] Artifact references — inline on the decision that rests on the evidence, plus a short reference block
      at the end of each spec file. No global index
- [ ] Mint tickets **only from the V1 rung**; everything below stays prose in the ladder
- [ ] The minted ticket inherits the artifact references attached to the section it came from
- [ ] The exit condition is the scope ladder, not "the thinking is done"

### 2d2 — A ticket is a FOLDER (settled 2026-08-06, reverses 08-05's flat-file pool)

```
docs/tickets/t047-daemon-detection/
  ticket.md          frontmatter + body + ## Plan.  Constant filename, like SKILL.md
  handoff.md         the resume, overwritten, at most one
  <slug>.md          a job brief per dispatched side-job — debugging, a parallel investigation
```

Modelled on the skills layout the user already lives with (`skills/brainstorm/SKILL.md`). **Uniform from
birth** — never a file that gets promoted, never a mixed directory, never a path that changes during the
ticket's life. The user's promote-on-`in-progress` variant was argued down and dropped: location would
duplicate `status`, and all six statuses would then need a location rule.

The agent's earlier objection — that a ticket with an "inside" recreates delapse's `m08e` nesting — is
**withdrawn**. It does not survive the parallel-dispatch case above, which needs real sibling files, and the
gravity well already has a designated exit: work that deserves decomposition becomes a **topic**, which is
the concept that exists for exactly that.

- [ ] Inner filename is a constant (`ticket.md`), not the slug repeated — tooling always knows the path
- [ ] `flow` commands create and move the folder; the shape is never assembled by hand
- [ ] Sweep every doc that says the pool is flat files — `design-brainstorm-rework.md` `## SESSION 2026-08-05`
      is the main one

### 2d3 — ❓ Scale: a few hundred tickets, most of them `done`

Raised by the user 2026-08-06 and not previously considered. At a real milestone the pool holds hundreds of
entries, overwhelmingly terminal.

- [ ] **Filtering must never require reading the pool.** `flow ls --status …` parses frontmatter across
      every ticket; that is fine for a script and impossible for an agent doing it by hand. This is the
      strongest argument yet that the commands are mandatory rather than convenience
- [ ] ❓ **Archive terminal tickets, or leave them?** `done` / `dropped` / `superseded` are absorbing states,
      so moving them to `docs/tickets/archive/` is a one-way move at end of life — a much weaker version of
      the location-duplicates-status objection than moving on `in-progress`. Real benefit: every glob and
      directory listing stops carrying hundreds of dead entries, and context is the scarce resource.
      **Recommendation: not yet.** Partitioning before it hurts is the failure this design keeps hitting;
      revisit with a real count

### 2e — 🔁 `write-plan.md` → the ticket's `## Plan` section

- [ ] Plan is a section **inside the ticket file**, written at pickup, never earlier
- [ ] Part 1 — examine the current state of what the ticket changes and record it (signatures, the seam,
      what surprised you)
- [ ] Part 2 — numbered steps, each naming the files it touches
- [ ] The code rule: *write the code that was decided, describe the code that follows from it*
- [ ] Parent tickets never get a plan
- [ ] The pickup judgment — unopened → broken-down (open a topic with `from: [t]`) or planned. This is the
      only real decision in the system, and it happens at pickup, never in advance
- [ ] Mid-build discovery, three outcomes: new ticket / rewrite the plan in place / `superseded` + `by:`
- [ ] Decomposition rule: every ticket finishable and checkable without its siblings, with an observable
      "done" written at creation. Wide refactors take expand → migrate → contract
- [ ] ⚠️ Carried open from 08-05: **nothing forces the "examine current state first" pass**, and it is the
      load-bearing half of the plan

### 2f — 🔁 `execute/SKILL.md`

- [ ] Reads the **ticket**, not `plan.md`; marks steps in the ticket
- [ ] Status transitions it owns: `todo` → `in-progress` → `review` → `done`
- [ ] Implementation only ever happens on a ticket with no children
- [ ] Haiku delegation and the debug-agent handoff survive as-is; `haiku-worker.md` path refs stay valid

### 2g — `code-review` skill — **promoted from optional to blocking**

`review` is a real status with a behavioural difference (it satisfies another ticket's `deps`), it is
**universal** rather than UI-conditional, and nothing implements it.

- [ ] Build it. Shape is already chosen: a reviewer subagent given base/head SHAs plus the requirements,
      returning strengths / issues / assessment (`reference/superpowers/skills/requesting-code-review/SKILL.md`)

### 2h — 🔁 `flow/global/CLAUDE.md` knock-ons

- [ ] `## Workflow` block still says `brainstorm.md → spec.md → plan.md`. Rewrite for the ticket chain
- [ ] `## Key docs` table is missing `docs/tickets/`, `docs/topics/`, `docs/brainstorm/` and `protos/`, and
      its `docs/spec/` row still promises "decisions"
- [ ] `## Capture` routes locked decisions to "the active topic's `spec.md`, or `docs/spec/decisions.md`" —
      **both are dead**. The brainstorm tree is the decision log now. (This closes the one open item in
      `design-capture-rework.md`)
- [ ] Add the `~/.claude/flow-notes.md` row. Routing test: *is this note about the thing I'm building, or
      about Flow itself?* Entries stamped date + project
- [ ] ❓ **Does `docs/work/backlog.md` still exist?** Raised by the user 2026-08-06: minting a ticket
      mid-work (id, frontmatter, topic, deps, a file) is heavy next to jotting one line, so killing
      backlog.md outright may have removed a real convenience.
      **Agent recommendation — drop it, and point `## Capture`'s future-work row at `docs/inbox.md`.**
      The inbox already *is* the zero-decision capture file ("fragments, half-formed ideas, anything with no
      home yet"), and its whole premise is that you do not route at capture time. Once work has a real home
      in the ticket pool, backlog's only surviving job is "work I have not committed to yet" — which is the
      same sentence as "not yet routed." `organize` gains one destination: **mint a ticket.** One staging
      file, one drain, one pool.
      *Rejected alternative:* keeping backlog.md as an explicit staging area that drains into tickets. That
      is the inbox with a second name, and it re-imposes a routing decision at capture time — the exact
      thing the inbox model was built to remove
- [ ] ❓ **Does `docs/work/` still earn a folder?** **Agent recommendation — no, delete it.** With
      brainstorm, topics and tickets all at `docs/` level it holds two transient files; and the name is now
      actively misleading, because the actual work lives in `docs/tickets/`. Move them to `docs/inbox.md`
      and `docs/handoff.md`. The per-topic variant `docs/topics/<slug>/handoff.md` is unaffected

### 2i — 🔁 stale paths in the other skills

- [ ] `organize/SKILL.md` — "route decisions to the topic's `spec.md`" (topics have no spec.md now); "skills
      mean the ones under `.claude/skills/`" (skills are global-only since #H1)
- [ ] 🔁 `handoff/SKILL.md` — **needs a new resolution ladder, not a path fix.** It currently knows two
      locations (`docs/work/topics/t<NN>-<slug>/handoff.md`, else `docs/work/handoff.md`); both paths are
      dead, and the user raised 2026-08-06 that **a handoff is needed per ticket too** — mid-implementation
      is exactly when a session runs out. Product brainstorms need one as well: they are multi-session by
      default, and that is what replaced real-aloud's 75-line resume blob.
      Proposed ladder, most specific wins:

      | when | where |
      |---|---|
      | implementing a ticket | `docs/tickets/t047-slug/handoff.md` |
      | a topic is active | `docs/topics/<slug>/handoff.md` |
      | the product brainstorm is active | `docs/brainstorm/handoff.md` |
      | none of the above | `docs/handoff.md` |

- [ ] **A resume handoff and a job brief are different things — the skill already says so, and tickets give
      the second one a home.** `handoff/SKILL.md` today: the default is *resume the same work*, overwritten,
      one per context; naming a different job outright ("debug this", "investigate in parallel") makes it a
      **brief** for that job, written as its own file so it cannot clobber the resume. Several briefs can be
      live at once; there is only ever one resume. With folder-per-ticket both land inside the ticket folder:
      `handoff.md` for the resume, `<slug>.md` per brief. Raised by the user 2026-08-06 as the case a single
      file could not serve — correct, and this is the case that earns the folder
- [ ] **Dispatching a subagent is an execution choice; minting a ticket is a recording choice. They are
      independent** (locked 2026-08-06). A tricky bug found while implementing t047 does **not** become a
      ticket merely because you want a separate session on it — write a brief and dispatch. It becomes a
      ticket only under the existing mid-build-discovery rule, when the work turns out to be genuinely
      separable. Otherwise it is churn: an id, a status, a deps entry, `done` twenty minutes later
- [ ] The debugging brief is what the unbuilt `debug` skill consumes — design the two together
- [ ] ~~Milestone-level handoff path~~ — **dissolved.** Milestones no longer exist; tickets replaced them

### 2j — `flow/project-template/`

- [ ] Depends on 2h's two open questions — what actually ships beyond `CLAUDE.md` and `.gitignore`

---

## Step 3 — `setup-flow-globals` (new skill)

Once per machine. Design: `design-init-flow.md` #G7.

- [ ] Writes `~/.claude/CLAUDE.md`, symlinks `~/.claude/scripts/`, merges `~/.claude/settings.json`
- [ ] Interviews once for `## The user`
- [ ] Existing populated `~/.claude/CLAUDE.md` → append under a marked Flow heading, never overwrite; report
      contradictions rather than silently competing
- [ ] **Never overwrites `~/.claude/flow-notes.md`** — the one file there that belongs to the user, not the
      template. Silent and unrecoverable if it does
- [ ] Registers the `flow` scripts globally (depends on 2a)
- [ ] Sets `autoMemoryEnabled: false` — currently `true` globally, with live memory dirs for `backmark` and
      `backmark-validation`

---

## Step 4 — `migrate-to-flow` (new skill)

Written last, against a destination that by then exists. The old `init-flow` SKILL.md is **rejected in full**
and parked at `new-workflow/rejected-init-flow/` — the input to the rewrite, never patched.

- [ ] Test built-in `/init` with `CLAUDE_CODE_NEW_INIT=1` against a real repo **first** — it already does
      subagent codebase exploration, gap questions and a reviewable proposal, which is most of the survey phase
- [ ] Decide the "already on Flow" marker — `## Workflow` is no longer in the project CLAUDE.md; presence of
      `docs/work/backlog.md` is the leading candidate, and 2h may kill that file
- [ ] Quarantine colliding paths, never merge (#10)
- [ ] Harvest into `CLAUDE.md` + `docs/context/` only — **never** `docs/spec/` (#12)
- [ ] Fetch payload by raw URL, no clone; delete only what the run created
- [ ] `.claude/agents|commands|skills` analyzed for **behavioural** collisions, reported, never moved silently
- [ ] Codebase survey reads real code — on disagreement the code wins and the stale doc claim is dropped

---

## Step 5 — the real migration

- [ ] Migrate `delapse` and `lumacraft_v2` — the real test cases, and the reason the migration path exists
- [ ] Delete their project-local skills afterwards (skills are global-only, one symlinked copy per machine)
- [ ] Harvest `delapse` / `lumacraft_v2` / `framework-build` knowledge into skills before that material is lost

---

## Skills — not blocking the chain

- [ ] **`debug` — the general systematic-debugging skill. Never designed, never built.** Raised again by
      the user 2026-08-06 and missing from the first version of this list. It was named as a needed skill
      back in the original problem inventory, and its core principle already shipped as a hard rule in
      `global/CLAUDE.md` (*"No cause without evidence. Hypothesis: X. To verify: Y."*) — but the rule is one
      line and the skill is the loop around it. Highest-value item in this group: it fires on every project,
      every stack, constantly.
      **Design it against `debug-web-pages` and against `reference/superpowers/skills/systematic-debugging`.**
      The disjointness rule applies to skills too — the general skill owns the *method*, the domain skill
      owns browser specifics. Today `debug-web-pages` is the only debugging skill Flow has and it silently
      stands in for a general one it was never scoped to be
- [ ] **`prototype` — brainstorm it before building it.** Product mode leans on it for three jobs
      (feasibility spikes, UI mockups, proof-of-concept builds) and what the skill *is* has never been
      decided. Un-parked 2026-08-06
- [ ] **A router skill, in the shape of `ask-matt`.** Value is the *narrative* — main flow, on-ramps,
      standalone skills, how they cross sessions — not the skill list. Ships with the rule that it is re-read
      and updated whenever a user-reachable skill is added, renamed or removed. A router that lies is worse
      than none
- [ ] **`testing` — decide, leaning no.** Stack-specific testing knowledge belongs in that stack's skill;
      project-specific conventions belong in `docs/context/`
- [ ] 🔁 **`explain`'s mockup output is weak** (user's verdict). The ASCII/structural side works; the mockup
      side needs its own pass
- [ ] 🔁 **Telegraphic refactor pass** over `brainstorm`, `research`, `execute`, `debug-web-pages`
- [ ] `debug-web-pages` is prototype-adjacent but narrow — revisit alongside the `prototype` brainstorm

---

## Hooks

- [ ] **`PreCompact` hook** — block-once state file, so auto-compaction gives way to `handoff`
- [ ] **Context-pulse hook** — inject remaining-context info so the agent can trigger `handoff` itself.
      The agent can often judge this unaided; the hook makes it reliable

---

## Housekeeping

- [ ] **Link the skills globally** once the set is final:
      `bash /home/me/code/projects/agentic-setup/flow/global/scripts/link-skills.sh`. Not before — nothing
      loads until then, and that is deliberate
- [ ] **Tune `guard.py`'s deny/ask lists** against real use. Written from the hard rules, never against
      observed false positives; a `deny` verdict cannot be overridden in-session
- [ ] **Empty the ~50-entry `allow` array** in `agentic-setup/.claude/settings.local.json` — dead exact-string
      rules for paths the #H1 restructure moved. **Awaiting explicit yes; it is a delete**
- [ ] **Real commit messages.** Every commit in `flow` and the workbench says `save`, which is why per-skill
      changelogs have to carry the reasoning
- [ ] **Rewrite `session-new-plugin.md`'s stale "Skills still to build" list**
- [ ] **Clean up `new-workflow/` redundant content** — deferred until the design lands; several docs now
      carry superseded blocks

---

## Design threads still open

- [ ] **Ecosystem branch #6 — the skill-creation trigger** (`design-skill-ecosystem.md`). #5 closed 2026-07-30
- [ ] **The review/finalize phase that triggers `organize`.** Partly answered by the ticket `review` status;
      the wiring was never designed
- [ ] **Revisit stack-skill recommendation at init** — the catalog holds eight process/domain skills and zero
      stack skills, which is why it was deferred
- [ ] **Audit** — checking current work against a skill's accumulated best practices. Parked as ecosystem
      issue #4; needs scoping to the skills the work actually touched, or it is unbounded reads
- [ ] **Red-team / grill mode** — an adversarial pass that attacks a design before it locks. Captured
      2026-07-23, never designed. Ancestor: delapse's `grill-me` / `grill-with-docs`

---

## Deferred deliberately — no action, wait for real cases

- Whether a leaf ticket is always plannable in ~35 lines. A ticket spanning a migration plus a backfill plus
  UI probably isn't
- A **fifth ticket type** for document-producing work that is not research. Bar: *it must change what the
  ticket produces, or be a filter you would actually run.* Watch for "design X" tickets — designing is not
  researching
- Splitting `~/.claude/flow-notes.md` — allowed **by kind, never by project**, and only once one file hurts

---

## Absorbed

`new-workflow/backlog.md` is fully represented above and is now a second copy of this list. It should be
deleted or reduced to a pointer — **the user's call, since it is a delete.**
