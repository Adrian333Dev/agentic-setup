# Workbench backlog

Flat list of loose items raised in session and not yet handled. Not ordered, not a commitment. Drain into design docs or into `flow/` / `flow-skills/` as they get picked up.

---

## Skills to build

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

## Docs / structure

- **Milestone-level handoff path** — where a handoff lives when a milestone rather than a topic is the unit.
- **Design the review/finalize phase that triggers `organize`.**
- **Rewrite `session-new-plugin.md`'s stale "Skills still to build" list.**
- **Ecosystem branches #5 and #6** in `design-skill-ecosystem.md` — the init-integration seam (partly answered by `init-flow`) and the skill-creation trigger.

## Migration

- **Migrate the user's own active projects to Flow once it's ready** — `delapse` and `lumacraft_v2` are the real migration test cases, and the reason the migration path has to work. Afterwards the externally installed skills get deleted from those projects.
- **Harvest `delapse` / `lumacraft_v2` / `framework-build` knowledge into skills** — the reference projects hold hard-won material that should not be lost when they migrate.

## Pending decisions

- **`reference/archived-skills/note/`** — still awaiting a delete decision. Committed there either way; git retains it.
