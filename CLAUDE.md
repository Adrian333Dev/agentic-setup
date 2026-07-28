# Agentic Workflow — Workbench

> This is the **incubator repo** for building the Agentic Workflow v2 project. It is **not** the template itself and **not** the skills catalog — those are two nested repos below. This root only holds them, the design work, and reference material.

## Layout

| Path | What it is |
|------|-----------|
| `flow/` | **The product** — the workflow template being built (own CLAUDE.md, docs scaffold, utility scripts). Own `.git`; future standalone repo. |
| `flow-skills/` | **The personal skills catalog** — installs via `npx skills add Adrian333Dev/flow-skills`. Process and knowledge skills reused across projects. Own `.git`. |
| `new-workflow/` | **Design lab** — the thinking behind v2: `design-*.md`, `hard-rules.md`, `session-new-plugin.md`, `research-log/`. Not shipped. |
| `reference/` | **Read-only reference** — `v1-template/` (archived old template), `framework-build/` (v1 build notes), and cloned external skill repos (`superpowers`, `mattpocock-skills`, `taste-skill`, …). Mine it; never edit as live. |
| `tmp/` | Scratch. Gitignored. |

`flow/`, `flow-skills/`, and the clones under `reference/` each carry their own `.git`. This root repo is the incubator around them — a temporary arrangement that gets untangled at the real repo-split later.

## To resume work

Read **`new-workflow/session-new-plugin.md`** — the master resume file: design branches, decisions locked, next action.

## Hard rules (this workbench)

- **Never run git mutations** (the deny list blocks them). Suggest the exact command; the user runs it. Applies to every repo here.
- **Propose a plan and wait for explicit approval before any file change.**
- **Never delete source after copying** without a separate explicit confirmation — even inside an approved plan. Moves into `reference/` are fine; deletes are not.
- **This meta-work uses plain conversational brainstorming** — do NOT invoke `superpowers:brainstorming` or the v2 `brainstorming` skill for designing the workflow itself.
- **`reference/` is read-only.** Never treat it as live source.
- **Never run install or setup commands** — `pnpm add`/`remove`, extensions, global CLIs, MCP servers, system packages. No exceptions; name the command, the user runs it.
- **Keep internal reasoning out of deliverables.** Rejected-alternatives / "deliberately skipped" catalogs belong in design or notes, never in a polished artifact.
- **Scratch files stay in-repo** (`tmp/`) or the session scratchpad — never root `/tmp`.
- **No auto-memory.** Anything worth keeping goes in the repo (this file, the session doc, or a skill), not the memory feature.

## Working with this user

- Solo developer. Communicates almost entirely by **voice-to-text**, so messages carry transcription errors — misspellings, wrong or dropped words, homophones, run-on phrasing. Read for intent, not literal wording; infer the intended word from context. Ask only when a likely mis-transcription genuinely changes the meaning and context can't settle it.
- **No fluff.** No cheerleading, no jargon, no filler, never "you're absolutely right." Every sentence earns its place. In brainstorming, write free-form prose (not compressed/telegraphic); telegraphic fragments are fine elsewhere.
- Works iteratively: commit to a recommendation so he can react, rather than laying out every option neutrally.

## Communication (/copy + comprehension)

- The user copies the **last message** of each turn with a `/copy` command. Put **all tool calls — reads and writes — before the final prose**, and make the full response the last thing in the turn. Never emit prose and then edit files after it.
- **Batch writes.** Write to working docs only when a decision is genuinely locked (no open threads on it), and let a few accumulate before recording them together — not every turn.
- **Explain artifacts from zero.** Never assume the user has read a research report or a file. Explain the relevant content in plain language — what it says, what you conclude, what you propose, why. Research reports get the strongest form: assume zero lines read. (Earlier chat messages are fine to assume read.)

## Not to be confused

- `flow/CLAUDE.md` — the **template's** own instructions (deferred; finalized only once its skills are complete). Different file, different job from this one.
- `flow-skills/CLAUDE.md` — the **skills catalog's** authoring guide.
