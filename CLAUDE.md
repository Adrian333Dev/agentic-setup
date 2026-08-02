# Agentic Workflow — Workbench

> This is the **incubator repo** for building the Agentic Workflow v2 project. It is **not** the product itself — that is `flow/`, a nested repo below. This root only holds it, the design work, and reference material.

## Layout

| Path | What it is |
|------|-----------|
| `flow/` | **The product** — the whole system, one repo since 2026-08-03: `global/` (installs to `~/.claude/`), `skills/` (the eight live skills, symlinked into `~/.claude/skills/`), `project-template/` (what a new project starts with). Own `.git`; future standalone repo. Not a GitHub template repo — cloned once per machine. |
| `new-workflow/` | **Design lab** — the thinking behind v2: `design-*.md`, `hard-rules.md`, `session-new-plugin.md`, `research-log/`. Not shipped. |
| `reference/` | **Read-only reference** — `v1-template/` (archived old template), `framework-build/` (v1 build notes), and cloned external skill repos (`superpowers`, `mattpocock-skills`, `taste-skill`, …). Mine it; never edit as live. |
| `tmp/` | Scratch. Gitignored. |

`flow/` and the clones under `reference/` each carry their own `.git`. This root repo is the incubator around them — a temporary arrangement that gets untangled at the real repo-split later.

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

## Explaining

Governs every answer — status reports and one-line questions included, not just designs.

- **Whole picture first.** The thing itself, then its parts. Never a close-up with no machine around it.
- **Define from zero.** Anything invented here — module, phase, term, file — defined before first use. No expertise covers what didn't exist yesterday.
- **No undefined shorthand.** "Consolidation", "the harvest", "the payload", "the seam", "#4" — ground it in what the user actually sees, or drop it. A term coined in an earlier session is still shorthand; define it again or use plain words.
- **Calibrate tech** against what the user knows. Unfamiliar: one line, by what it does here.
- **Priority order.** The load-bearing idea gets depth — the why, and why the obvious alternative fails. Trivia gets one line or none.
- **The final message is written for the user.** Design docs and session files are for agents; the user skims them at most. Never let one stand in as the answer.
- **Outline before typing.** Never discover the structure on the way.
- **No preamble.** Content starts at sentence one.

## Communication (/copy + comprehension)

- **Reason before agreeing.** Test a proposal, objection, or correction — don't just accept it. Disagree out loud, with the argument, once. Repetition isn't evidence. Then the user decides.
- The user copies the **last message** of each turn with a `/copy` command. Put **all tool calls — reads and writes — before the final prose**, and make the full response the last thing in the turn. Never emit prose and then edit files after it.
- **Batch writes.** Write to working docs only when a decision is genuinely locked (no open threads on it), and let a few accumulate before recording them together — not every turn.
- **Explain artifacts from zero.** Never assume the user has read a research report or a file. Explain the relevant content in plain language — what it says, what you conclude, what you propose, why. Research reports get the strongest form: assume zero lines read. (Earlier chat messages are fine to assume read.)

## Not to be confused

- `flow/CLAUDE.md` — instructions for working **on the flow repo**. Different file, different job from this one.
- `flow/global/CLAUDE.md` — the **template version** of the rules that install to `~/.claude/CLAUDE.md`. Public, placeholders only — never put personal profile content in it.
- `flow/skills/CLAUDE.md` — the **skill authoring guide**.
- `flow/project-template/CLAUDE.md` — what a **new project** starts with: `## Project` + `## Project rules`, nothing else.
