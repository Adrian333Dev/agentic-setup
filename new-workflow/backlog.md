# Workbench backlog

> **SUPERSEDED 2026-08-06 by `new-workflow/remaining.md`**, which is the single master checklist for
> everything left to build — every item below is represented there, with build order and design pointers.
> This file is now a second copy and should be deleted; awaiting the user's yes, since it is a delete.

Flat list of loose items raised in session and not yet handled. Not ordered, not a commitment. Drain into design docs or into `flow/` as they get picked up.

---

## Skills to build

- **`prototype` — brainstorm it before building it (raised 2026-08-06).** Un-parked. It was parked on 2026-07-30 as "the real need is feasibility spikes," but the product-mode design now depends on it for three different things: technical feasibility spikes (`protos/tts-lab/`), UI mockups, and proof-of-concept builds. What the skill actually *is* has never been decided — that brainstorm comes first, the build second.
- **Our own `code-review` skill.** Named as a future build, never designed.
- **A router skill in the shape of `ask-matt`.** Its value is the *narrative* — main flow, on-ramps, standalone, vocabulary layer, crossing sessions — not the skill list. Flow has nothing that says how its skills relate. Build it for the author first; newcomers benefit as a side effect. Ships with the maintenance rule: whenever a user-reachable skill is added, renamed, removed, or changes how it fits, the router is re-read and updated. A router that lies is worse than none.
- **A `testing` skill — undecided, leaning no.** Raised because `docs/agents/testing.md` exists in both reference projects. User's own read: sketchy, and if built it would have to be very minimal and generic. Recommendation on the table: don't. Stack-specific testing knowledge belongs in a catalog skill for that stack; project-specific testing conventions (verification order, what E2E covers here) belong in `docs/context/`, where `execute` reads them.

## Skills to rework

- **`explain`'s mockup output is minimal and not good** (user). The ASCII/structural side works. The mockup side needs its own pass.
- **`debug-web-pages` is prototype-adjacent** but very narrow — revisit when the feasibility-spike question is reopened (`design-init-flow.md` #8).
- **Telegraphic refactor pass** over `brainstorm`, `research`, `execute`, `debug-web-pages`.

## Hooks

- **`PreCompact` hook** — block-once state file, so auto-compaction gives way to `handoff`.
- **Context-pulse hook** — inject remaining-context info so the agent knows when its context is large and can trigger `handoff` itself. The user believes the agent can often judge this unaided (after a brainstorm closes, after a spec or plan is written, after execution finishes), but the hook makes it reliable.

## Scripts

- **The ticket/topic scripts are named everywhere and designed nowhere (raised 2026-08-06).** `## SESSION 2026-08-05` locked "commands, not an index file" — `flow tickets --ready`, `flow ticket new`, a status command — and then never specified them. Needed: what each one does, where the script lives, how it is invoked, and **global registration** so both the agent and the user can call it from any project without a path. Also unresolved from 08-05: whether `flow ticket new` being mandatory for id assignment is enforceable at all. This blocks the ticket system being usable, not just tidy.

## Flow-notes plumbing (locked 2026-08-06, not yet built)

- **Add the `~/.claude/flow-notes.md` row to `## Capture`** in `flow/global/CLAUDE.md`. Routing test: *is this note about the thing I'm building, or about Flow itself?* Flow → `~/.claude/flow-notes.md`; every existing row unchanged. Entries stamped date + project.
- **`setup-flow-globals` must never overwrite `~/.claude/flow-notes.md`.** It is the one file in `~/.claude/` that belongs to the user, not the template. A re-run destroying it is silent and unrecoverable. Write the exclusion into the skill when it is built (build step 3).
- Design rationale in `design-brainstorm-rework.md` → `## SESSION 2026-08-06` → *Notes about Flow itself*.

## Docs / structure

- **Milestone-level handoff path** — where a handoff lives when a milestone rather than a topic is the unit.
- **Design the review/finalize phase that triggers `organize`.**
- **Rewrite `session-new-plugin.md`'s stale "Skills still to build" list.**
- **Ecosystem branch #6** in `design-skill-ecosystem.md` — the skill-creation trigger. (#5 closed 2026-07-30.)
- **No Flow skills are linked globally (2026-08-03).** The five stale `~/.claude/skills/` links into `flow-skills/` were removed; `~/.claude/skills/` now holds only the three unrelated `.agents/` links. **Deliberate — the skill set is not finalized, so nothing gets linked until it is.** Until then no Flow skill loads in any project, and `flow/skills/` is edited as plain files. When ready: `bash /home/me/code/projects/agentic-setup/flow/global/scripts/link-skills.sh`. Do not link earlier "for convenience."
- **`autoMemoryEnabled: true` in `~/.claude/settings.json`.** Auto memory is on globally and has written live memory dirs for `backmark` and `backmark-validation`. Flow's own settings already set it false. Flips when `global/settings.json` gets merged in — which happens only once the repo is finalized (`design-init-flow.md` #G5, #G8, #H9).
- **Tune `guard.py`'s lists against real use.** Its deny/ask sets (`design-init-flow.md` #H9) are a first pass written from the hard rules, not from observed false positives. A `deny` verdict cannot be overridden in-session, so anything that fires wrongly has to be fixed in the file. Review after the first stretch of real work under it.
- **Empty the ~50-entry `allow` array in `agentic-setup/.claude/settings.local.json`.** Dead weight — exact-string rules for commands whose paths the #H1 restructure moved. Awaiting the user's explicit yes; it is a delete.
- **Commit messages.** Every commit in `flow` and the workbench says `save`, which is why the per-skill changelog has to carry the reasoning (`design-init-flow.md` #H5). Real messages would make most of that free.
- **Decide the "already on Flow" marker** for `migrate-to-flow`. `## Workflow` left the project `CLAUDE.md` under the global split, so the old detection breaks. Presence of `docs/work/backlog.md` is the leading candidate. Trivial; unrelated to the retired divider (#H4).
- **Test built-in `/init` with `CLAUDE_CODE_NEW_INIT=1`** against a real repo before writing `migrate-to-flow`'s survey phase — it already does subagent codebase exploration, gap questions, and a reviewable proposal.
- **Revisit stack-skill recommendation at init** once the catalog holds any stack/tool skills — today it holds eight process/domain skills and zero stack skills, which is why ecosystem #5 deferred it.

## Migration

- **Migrate the user's own active projects to Flow once it's ready** — `delapse` and `lumacraft_v2` are the real migration test cases, and the reason the migration path has to work. Afterwards their project-local skills get deleted: skills are global-only now, one symlinked copy per machine, never copied into a project (`design-init-flow.md` #H1).
- **Harvest `delapse` / `lumacraft_v2` / `framework-build` knowledge into skills** — the reference projects hold hard-won material that should not be lost when they migrate.
