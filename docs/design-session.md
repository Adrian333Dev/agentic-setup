# Framework Design Session — Complete Locked Decisions

**Sessions:** 2026-06-24, 2026-06-25, 2026-06-26
**Status:** Design extended. More decisions locked. Still in planning — nothing built yet.
**Goal:** Design and build a custom agentic workflow framework to replace Superpowers entirely.

---

## Why We're Here

Superpowers failed on the Delapse project:
- Kept suggesting/executing git commands despite explicit overrides
- Kept recommending `subagent-driven-development` against explicit prohibition
- Too opinionated — enforced workflow that didn't fit actual working style
- Override mechanism (`superpowers-overrides.md`) was a constant patch layer, not a real fix
- Debugging was terrible without manual intervention
- Context explosion at session start — ~90-100K tokens consumed before doing any work
- After context compaction, skills re-applied as if overrides didn't exist
- Milestone-based planning created waste when plans became obsolete during discovery
- Offered visual companions, worktrees, and forced steps that were not wanted

**Decision: Abandon Superpowers completely. Build a custom framework from scratch.**

---

## Current State of Work

- Design session 1 (2026-06-24): Locked decisions 1–25. Grilling complete.
- Design session 2 (2026-06-25 to 2026-06-26): Deep-dived on /brainstorm skill design. Locked decisions 26–34.
- Nothing has been built yet. One premature implementation attempt was rejected and reverted by user.
- Next: finish planning remaining skills (/write-spec, /write-plan, /debug, /checkpoint), then build in subdirectory.

---

## All Locked Decisions

### 1. Overall philosophy

Light structure, not zero structure and not full structure. Key principles:
- Skills are **user-invoked only** — never auto-trigger
- Agent never insists, enforces, or suggests the next step unless asked
- No mandatory workflows
- Flexibility is the top priority
- **Every design decision optimizes for cost efficiency** — no wasted tokens, no redundant reads

### 2. Git policy — absolute prohibition

Agent **never** runs, suggests, mentions, or offers any git write command: `git add`, `git commit`, `git push`, `git pull`, `git merge`, `git rebase`, `git reset`, `git checkout`, `git stash`, `git clean`, `git branch`, `git worktree`, `gh pr create`, `gh pr merge`, or any variant.

Read-only git is fine: `git diff`, `git log`, `git status`, `git show`.

User may optionally stage files mid-session at their own discretion — agent never suggests this. Committing is always the user's act of approval.

**Enforcement:** hook-level via `git-guardrails-claude-code` pattern, installed globally. Not just a CLAUDE.md text rule — technical block.

### 3. No TDD

Implement first, then write tests. Tests are non-standard and varied. No TDD skill needed.

### 4. No subagent-driven-development

Single session context always preferred. No SDD equivalent.

### 5. Never use AskUserQuestion tool

Never use the structured question/option UI tool. Ask in free-form text instead — any number of questions, freely worded. The UI tool blocks workflow.

### 6. Three-tier context architecture

**Purpose:** Keep session-start token cost under 10K total. Superpowers used 90-100K before doing any work.

**Tier 1 — Always-in-context (CLAUDE.md):**
- ≤80 lines (~2K tokens)
- Project name, stack, structure (brief)
- Hard rules (git prohibition, non-negotiables, behavioral rules)
- Session start instructions
- Routing table: "if working on X, read docs/conventions/Y.md"
- Universal coding conventions (short enough to inline)

**Tier 2 — Session-start reads (every session and after compaction):**
- `docs/work/now.md` — active milestone + folder path
- `docs/work/milestones/<slug>/session.md` — if active milestone has one

That's it. No workflow-rules.md, no separate conventions at session start.

**Tier 3 — Situational reads (routing table in CLAUDE.md triggers these):**
- Stack-specific conventions — separate files, read when working in that area
- `docs/agents/commands.md` — only when about to run commands
- Milestone `spec.md` — read when starting work on a specific milestone
- Domain KB docs — when relevant to the work

CLAUDE.md is a routing table, not a dump of all conventions.

### 7. Behavioral hard rules (in CLAUDE.md)

**Two-strike rule:** After two failed attempts to fix the same issue (especially browser/UI), stop. Don't keep changing code and retrying variations. Write a stuck brief (what was tried, evidence, hypothesis, proposed next step) and discuss with the user.

**Scope discipline:** Agent only touches files explicitly within the current task scope. No opportunistic cleanup, no "while I'm here" refactors. If something outside scope needs fixing, note it in `issues.md` or the backlog and continue.

**No redundant reads:** Never read the same file twice in a session. Never run multiple `ls/find` commands for what could be one tree call.

**Auto-create parent dirs:** Write tool creates parent directories automatically. Never run `mkdir` as a separate step.

**Concise writing:** Every word earns its place. Documentation and responses should be informative but worded concisely — not padded, not verbose.

### 8. Project-level documents

**Product spec** (`docs/spec/product-spec.md`): What the product does, who it's for, v1 scope, v2 scope, what's deferred. Living document — evolves slowly but does evolve.

**Tech spec** (`docs/spec/tech-spec.md`): Tech stack, architecture decisions. Initially a validation artifact ("is this feasible, with what tools?"). Shifts twice: once when the product shifts, once when deeper research changes understanding. NOT final on first write.

**Decisions log** (`docs/spec/decisions.md`): Project-level architectural decisions that span milestones. Format: what was decided, why, what was rejected, what was superseded (and when). This is the most durable artifact — in 6 months you need to know WHY a decision was made, not just what it was.

These are project-level, created once per project during the design phase. Not milestone artifacts.

### 9. Project-level design phase (greenfield)

A separate phase that runs BEFORE any milestone exists. Uses brainstorm/grilling sessions but isn't scoped to a single milestone.

**Flow:**
1. Product brainstorm sessions → `docs/spec/product-spec.md` + `docs/spec/decisions.md`
2. Technical research/validation sessions → `docs/spec/tech-spec.md` (embedded in design sessions, not a separate phase)
3. "What to build first?" session → looks at product spec, proposes first milestone scope → creates `docs/work/now.md` + `docs/work/milestones/m01-<slug>/`
4. First milestone → normal milestone flow begins

Technical research is embedded in design sessions. When a technical unknown surfaces, either: resolve inline (small unknown), or run a dedicated research spike session (large unknown), then return to the design session with new information.

This phase can take multiple sessions. Delapse took three full design sessions before any code was written.

### 10. Milestone structure

Milestones are **emergent** — decided at session start based on what to work on next. No upfront classification ("build" vs "explore" — complexity reveals itself during execution).

Each milestone gets its own folder: `docs/work/milestones/<slug>/`
Sub-milestones also get their own folders: `m08a`, `m08b`, etc. — NOT just categories in the parent.

**Standard folder contents:**
- `spec.md` — required
- `plan.md` — required (after brainstorm)
- `brainstorm.md` — written at start of every /brainstorm session, maintained throughout (see Decision 29)
- `session.md` — created once work begins, updated at natural breakpoints and end of session
- `issues.md` — optional, for bugs/deferred items discovered during implementation

### 11. Discovery handling

**Small change** (a few edits, doesn't require re-thinking the approach): inline correction, note it in `session.md` or `issues.md`, continue.

**Large change** (requires brainstorming + new decisions + rebuilding): stop current work, create a new focused sub-milestone with its own spec + plan, work through it fully, then resume the original. This is not a patch — it's a real sub-milestone.

When a previously completed branch needs to be completely rethought (e.g., Delapse m08: while working on branch B, discovered branch A needed a full redesign): go back, brainstorm branch A afresh, make new decisions, rebuild, then resume branch B with a solid foundation.

### 12. Spec format

```
## Goal
One sentence.

## Motivation
Why this, why now.

## Scope
In: ...
Out: ...

## Decisions
Design + implementation choices locked before coding.

## User Stories
(Only when user-facing — skip for internal tooling, eval packages, backend services.)
As a [actor], I want [feature], so that [benefit].

## Done-When
Concrete, testable criteria — specific observable outcomes, not "feature complete."

## File Layout
(Optional — only when structure is non-obvious or being created from scratch.)
```

### 13. Plan format

The plan skill reads **every file it's going to touch** before writing a single task. Output is per-file specific changes grounded in actual file content.

Not: "implement auth middleware"
Yes: "in `src/background/message-handler.ts`, add handler for `TIMESTAMPS_EXTRACTED` in the switch at line 34, call `storage.save(payload.timestamps)` — storage already imported at line 5"

This prevents mid-implementation loops caused by plans that don't match what's actually in the code.

### 14. Brainstorm → spec → plan flow

Three user-invoked skills, called in sequence when ready. No auto-progression.

**`/brainstorm`** — See Decision 27 for full session structure. Short summary:
- Never use AskUserQuestion tool
- Free-form questions and answers
- Significant questions get their own exchange; minor ones can be grouped
- Agent provides its recommended answer with every question — user reacts
- Writes and maintains `brainstorm.md` (decision tree working memory) throughout
- Ends when user decides they're done
- At close: summarizes locked decisions, suggests /write-spec

**`/write-spec`** — synthesizes the brainstorm conversation + brainstorm.md into `spec.md` using the locked spec format above. No interview — just synthesis.

**`/write-plan`** — reads `spec.md` + every file it'll touch → writes per-file change plan to `plan.md`. Must read actual code before writing any task.

### 15. Knowledge base — domain-specific living files

**NOT** generic category files (`debugging.md`, `patterns.md`). Too broad to be useful.

**YES** domain-specific and context-specific files. Named by domain + context, not artifact type:
- `docs/kb/prompts/eval-strategies.md` — prompt patterns, what worked/didn't, model comparisons
- `docs/kb/stack/zod-patterns.md` — Zod best practices, bad patterns, gotchas from real use
- `docs/kb/debugging/youtube-ui-extension.md` — YouTube DOM debugging in extension context specifically
- `docs/kb/product/ideas.md` — future directions, cost strategies, untried approaches

Living files — start small, grow over time. Referenced by CLAUDE.md routing table.

**KB writes are autonomous and immediate** — agent writes to the appropriate KB file the moment it recognizes something worth saving. Not at session end. Not batched. Right when the information surfaces. Brief inline note ("[saved to docs/kb/prompts/eval-strategies.md]"), then continue. User reviews and edits later.

What gets saved: prompt construction lessons, model comparisons, stack-specific gotchas, debugging steps that worked, optimization tactics, product direction ideas, API-specific behavior discovered during implementation, TypeScript/tooling issues and their fixes.

### 16. Backlog

No `roadmap.md`. Instead: `docs/work/backlog.md`.

- Flat, no categories, no ordering, no status
- Completely open-ended — items can be one-liners or multi-paragraph
- Anything: feature ideas, future technical work, models to test, optimizations to try, distribution experiments, cost reduction ideas
- Agent adds to it mid-session when relevant ideas surface — immediately, not at session end
- No commitment implied by presence on the backlog

### 17. `now.md` format

Minimal. Only job: tell the agent which milestone is active and where to find it.

```markdown
# Active Milestone

**Name:** <name>
**Folder:** docs/work/milestones/<slug>/
**Goal:** <one line>

## Completed
| Milestone | Folder |
|---|---|
```

"Next action" field: only included if the user explicitly wrote it when closing the previous milestone. Agent never invents it.

### 18. Session continuity

**`session.md` per milestone:** Written at natural breakpoints during a session and at session end. Format adapts to the session type — not a fixed implementation-biased template. See Decision 33 for the adaptive format.

**The /checkpoint skill:** When explicitly invoked by user, writes session state to the active milestone's `session.md`. Adaptive output — agent answers "what would someone need to read to continue this session without having been here?" and writes that. Code context section only appears if there's code to capture.

**Resume after long break:** No special flow. Just ask the agent to summarize the project state from existing docs. That's sufficient.

### 19. Verification

No separate verification skill. Done-when sequence:
1. Agent completes all tasks in `plan.md`
2. Agent runs the validation commands agreed at session start
3. Agent self-checks against done-when criteria in `spec.md`
4. User manually tests
5. Move on

### 20. Validation commands

Not a fixed script. Context-dependent:
- Depends on which part of the codebase the agent is working in (e.g., eval package only vs. extension vs. extension + backend)
- Tests are selective and expensive (15-20 seconds) — only run what's relevant to what changed
- Defined at the start of implementation (before the plan phase begins)
- Stored in the milestone's session.md or plan.md

Agent and user agree upfront: "for this task, run X." Not hardcoded, not automated.

### 21. Scripts and tooling

**Tree generation** — clean bash script, outputs filtered project tree to stdout. One call instead of multiple `ls/find`. `scripts/tree.sh [path]`.

**Merge command** — merge files/folders into a single stdout block with fenced code sections. `scripts/merge.sh path1 path2 ...`. One Bash call = all files read.

**Scripts design principles:**
- Clean and readable — simple bash
- NOT the `node scripts/run.js <command>` runner pattern
- Output directly to stdout — no intermediate file that needs a second read
- Context-dependent: different commands for different workspaces

### 22. Skills — what we adopt vs. build

**Adopt from mattpocock-skills (with adaptation):**

| Skill | Adaptation |
|---|---|
| `diagnosing-bugs` | Adopt as-is. Phase 1 (feedback loop first) is exactly what was missing. Remove CONTEXT.md/ADR refs, replace with session.md + KB files. |
| `git-guardrails-claude-code` | Set up globally (all projects). Hook-level enforcement. |
| `improve-codebase-architecture` | On-demand. Use when codebase feels messy. |
| `prototype` | Throwaway code to answer a design question. |

**Build custom (in our subdirectory, not the mattpocock repo):**
- `/brainstorm` — decision-tree grilling with brainstorm.md working memory
- `/write-spec` — synthesize brainstorm.md + conversation → spec.md
- `/write-plan` — read codebase → file-by-file plan.md
- `/checkpoint` — adaptive session.md writer (not the mattpocock handoff format)

**Borrow template, skip the skill:**
- `to-prd` — too tightly coupled to mattpocock's issue tracker. Borrow spec format inspiration only.

**Skip entirely:**
- `tdd`, `resolving-merge-conflicts`, `to-issues`, `triage`, `setup-matt-pocock-skills`, `decision-mapping`

**UI skills (optional, project-level, not in core framework):**
- `taste-skill` — anti-slop frontend. GSAP dependency concern. Try on real UI task before committing.
- `ui-ux-pro-max-skill` — 161 design rules. Python dependency concern. Auto-activates (needs override).

### 23. Three framework scenarios

**Greenfield (new project from scratch):**
1. Product design phase (multiple sessions) → `docs/spec/product-spec.md` + `docs/spec/tech-spec.md` + `docs/spec/decisions.md`
2. "What to build first?" session → first milestone scoped → `docs/work/now.md` created
3. Milestone flow begins (repeating cycle)

**Migration (existing project, like Delapse):**
Not a redo — a mapping + audit session:
- Product spec → keep, move to `docs/spec/product-spec.md`
- Decisions log → keep as-is
- Roadmap → rename/move to `docs/work/backlog.md`
- Technical research file → review for staleness: locked decisions → tech spec, learnings → KB files, unresolved → backlog
- CLAUDE.md → rewrite from new framework template
- `now.md` → update to new minimal format
- Existing milestone folders → keep or archive completed ones

**Milestone flow (repeating cycle):**
1. User picks next thing to work on (from backlog or they know)
2. `/brainstorm` → decisions locked, brainstorm.md maintained throughout
3. `/write-spec` → `spec.md`
4. Agree on validation commands for this task
5. `/write-plan` → `plan.md` (reads all relevant files first)
6. Implement (agent follows plan, writes to KB immediately when something worth saving surfaces)
7. Verify (plan complete + validation commands + spec done-when + user manual test)
8. `/checkpoint` → `session.md` updated
9. Update `now.md` completed table
10. Pick next milestone

### 24. Code review

Solo dev. No formal code review step by default. `/code-review` available on demand when wanted. Not part of the standard milestone flow.

### 25. Packaging

Deferred to end. The framework will have:
- Custom scripts
- Possibly hooks and settings
- Skills (in `.claude/commands/` as markdown files)

Decide structure and packaging after we know everything it contains.

---

### 26. Build location — subdirectory approach

New framework files go in a separate subdirectory during development. Do NOT write into the live project directories. Once the full design is approved and all files are ready, move them into the correct live positions.

Subdirectory location TBD when we start building (something like `temp/framework-build/` or `docs/framework-draft/`).

### 27. /brainstorm — session structure (4 phases)

A brainstorm session has four phases:

**Phase 0 — Context read** (only when relevant):
- Milestone or technical brainstorm: agent reads the relevant parts of the codebase or spec that bear on what's being designed. Not everything — just what's relevant to the decisions being locked.
- Product-level brainstorm (before any milestone or code exists): skip this phase.

**Phase 1 — Open:**
Agent states what it understands as the goal and identifies the main branches to work through. Not a formal hypothesis — just orientation. Then writes the initial brainstorm.md branch tree (top-level branches only at this point).

**Phase 2 — Walk the decision tree:**
For each significant branch: agent poses the question AND gives its recommended answer. User reacts (agree / disagree / "yes but also..." / "actually it's more like..."). Agent folds in the response and updates brainstorm.md. Moves to next branch.

Minor questions can be grouped: "Three small things to lock: [list]. Here's my take on each: [takes]. Anything you'd change?"

New sub-branches that emerge during conversation get added to brainstorm.md immediately.

**Phase 3 — Assumption check:**
Before closing, agent names key assumptions being made that haven't been validated: "We're betting that X is true. If it's not, we'd need to rethink Y." User decides whether to treat these as blockers or note them in backlog.

**Phase 4 — Close:**
User says they're done. Agent gives a concise summary of locked decisions. Suggests /write-spec next.

**Concrete example — milestone brainstorm: "add hotkeys to the extension"**

Agent opens: "Here's what I understand: configurable hotkey that triggers the card popup. Main decisions are (1) where hotkey listening lives architecturally, (2) default key, (3) settings UX, (4) storage, (5) edge cases. Let me work through each. First: content script or background? My take: content script — background workers can't see keyboard events on the page. Agree?"

User: "Content script is right. But we need to not trigger if the user is typing in YouTube search."

Agent updates brainstorm.md (adds sub-branch: input guard), responds: "Right — check `document.activeElement` against input/textarea/contenteditable before handling. Standard pattern. Locked. Next: default hotkey. Alt+C — not grabbed by Chrome or YouTube. Alt+Space conflicts with some OSes. Alt+A is YouTube autoplay. Alt+C feels safe. Any objection?"

...session continues branch by branch.

### 28. /brainstorm — agent commits to a position

For every question, the agent provides its recommended answer. Not "what do you think about X?" but "here's what I'd do for X, and why — does this hold?"

Rationale (from mattpocock's grilling mechanic): reacting to a guess is faster than generating an answer from scratch. Agent commits to a position; user confirms or corrects. This is what makes the grilling approach productive.

The agent should be willing to be visibly wrong. If the user pushes back, agent reconsiders genuinely — doesn't just capitulate, but doesn't defend a wrong answer either.

### 29. brainstorm.md — working memory file

Every brainstorm session starts by writing a branch tree file before asking any questions. Maintained throughout by agent judgment (not mechanical updates — agent decides when something branch-relevant happened).

**Location:**
- Milestone brainstorm: `docs/work/milestones/<slug>/brainstorm.md`
- Product-level brainstorm (before any milestone): `docs/spec/brainstorm.md`

**Format:**
```markdown
# Brainstorm — [Topic]
_Started: [date]_

## Branches

- [x] **Listener architecture**
  - Decision: content script (background workers can't see keyboard events)
  - [x] Input guard: check activeElement — not input/textarea/contenteditable
  
- [ ] **Default hotkey**
  - Candidate: Alt+C (not grabbed by Chrome/YouTube/common OSes)
  - [ ] Confirm no YouTube player internal conflicts
  
- [ ] **Settings UX**
  - [ ] Location in settings panel
  - [ ] Key capture mechanism (keydown handler during assignment?)
  - [ ] Conflict detection behavior
  
- [ ] **Storage**
  - [ ] chrome.storage.sync vs local
  - [ ] Storage key name and default value

- [ ] **Edge cases**
  - [ ] Fullscreen mode
  - [ ] Iframe embeds
```

**Update trigger:** Agent decides when to update — new branch discovered, branch fully resolved, user answer reveals a sub-branch, important new constraint surfaces. NOT after every exchange.

**Self-sufficiency after compaction:** [x] items include the decision AND reasoning inline, not just a checkmark. Reading brainstorm.md after compaction restores full decision tree state — no need to reconstruct from conversation history.

**What /write-spec uses:** brainstorm.md is the primary input to /write-spec. The skill reads this file (plus conversation context) instead of reconstructing decisions from the full conversation history.

### 30. Three-layer context capture pattern

Context capture operates at three layers, all triggered by agent judgment:

**Layer 1 — Session working memory (session-phase-specific):**
- During brainstorm: `brainstorm.md` — decision tree, updated continuously
- During implementation: relevant code state captured in session.md
- During research/debugging: key findings captured in session.md

**Layer 2 — Durable knowledge (immediate, any phase):**
Triggers:
- A prompt pattern worked or failed → write to `docs/kb/prompts/<topic>.md`
- A debugging approach resolved something non-obvious → write to `docs/kb/debugging/<domain>.md`
- A stack-specific gotcha was discovered → write to `docs/kb/stack/<lib>.md`
- A product direction idea came up → write to `docs/work/backlog.md`
- A project-level architectural decision locked → write to `docs/spec/decisions.md`

Key rule: **write immediately when it surfaces, not at session end.** Brief inline note, then continue working.

**Layer 3 — Session state checkpoint:**
`session.md` — written at natural breakpoints (major phase completed, before a risky change) AND at session end. See Decision 33.

### 31. Immediate capture rule

Write to KB files, backlog.md, decisions.md when important information surfaces — not at session end.

Why: agents tend to defer saves to session end, then compaction happens, session ends abruptly, or the intention to save gets lost. By the time "session end" arrives, the window to capture has often closed.

Mechanics: agent writes in-flow, adds a brief inline note ("[saved to docs/kb/prompts/eval-strategies.md]"), and continues. No interruption. User reviews and edits later.

### 32. Post-compaction reading rule

At session start and after compaction: read `now.md` + active `session.md` only. Do not spray-read to orient.

The spray-read problem: after compaction, agent "feels" like it needs to re-read everything (spec.md, plan.md, conventions.md, source files) and burns 70-80K tokens before doing anything useful.

The fix: session.md should be self-sufficient for resuming. Read other files only when a specific action requires them — not for orientation.

As the session continues beyond the start, the agent reads whatever files it needs for the work at hand. The restriction is specifically to the orientation phase after compaction or at session start.

### 33. session.md is adaptive

session.md captures whatever type of session this was. Format adapts to the session type. Not implementation-biased, not a fixed template.

**Examples by session type:**

Implementation session:
- Task state (completed, in-progress, not started from plan.md)
- Code context: exact snippets for what the next session will need to read or extend
- Resume: specific first file to open, first edit to make
- Open blockers

Research session:
- What was researched and where
- Key findings, what was learned
- What's still unresolved
- Where to look next, what to try

Brainstorm session:
- Where the decision tree stands (brief summary — brainstorm.md has the full detail)
- What's locked, what's open
- What the next session should do (continue brainstorm? move to /write-spec?)

Design/planning session (like this one):
- Decisions made this session with context
- What's still open
- Proposed next steps

Debugging session:
- What was tried, exact commands and results
- Current best hypothesis
- What to try next
- Any important discoveries about the codebase

**The guiding question for writing session.md:** "What would someone need to read to continue this session without having been here?" Write that. Nothing more.

### 34. /checkpoint skill is not rigidly templated

The /checkpoint skill asks the agent to answer one question: "What would someone need to read to continue this session without having been here?"

Then write that. The output format adapts to session type (see Decision 33). There are no mandatory sections. Code context section only if there's code context worth capturing. Resume steps only if there's a specific next action.

The existing checkpoint skill in `.agents/skills/checkpoint/SKILL.md` has good bones but is too implementation-biased. The section structure (Completed / In progress / Not yet started / Code context) should be treated as optional scaffolding for implementation sessions, not as a required format for all sessions.

---

## Skill Research Findings (from 2026-06-25 session)

Analyzed these existing skills to inform /brainstorm design:

**mattpocock `grilling`** (5 lines, the backbone of our /brainstorm):
> "Interview me relentlessly about every aspect of this plan until we reach a shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one. For each question, provide your recommended answer. Ask the questions one at a time, waiting for feedback on each question before continuing. If a question can be answered by exploring the codebase, explore the codebase instead."

This is the core mechanic. We take: walk each branch, commit to a recommendation per question. We adapt: allow grouping of minor questions.

**superpowers `brainstorming`** (heavy, most of it is bloat):
- Good: scope check — "Before asking detailed questions, assess scope: if the request describes multiple independent subsystems, flag this immediately."
- Good: the HARD-GATE concept (don't implement until design approved) — we have this implicitly (skills are user-invoked)
- Bad: AskUserQuestion preference, mandatory tasks/checklists, auto-invoking next skill, committing to git mid-session, visual companion, rigid one-at-a-time rule

**agent-skills `interview-me`** (deeper intent extraction):
- Hypothesis + confidence approach: state what you think the goal is before asking anything
- "What would you actually want if you didn't have to justify it?" — catches sophistication-signaling answers
- 95% confidence stop test: can I predict the user's reaction to the next 3 questions?
- We borrow: assumption check at end, willingness to be wrong about guesses
- We don't use: the rigid hypothesis/confidence/restate structure (too formal for milestone brainstorms)

**agent-skills `idea-refine`** (divergent thinking):
- Explicit assumption surfacing: name what you're betting is true but haven't validated
- "Not Doing" list — explicit exclusions
- We borrow: the assumption check at end (Phase 3 of our brainstorm)

**What we DO NOT take from any existing skill:**
- AskUserQuestion tool
- Multiple choice preference
- Auto-invoking next skill
- Writing docs mid-session (brainstorm.md is working memory, not a "doc")
- Git commits as part of the skill
- Rigid step-by-step checklists
- Visual companion / browser mockups

---

## Session Start — Correct Behavior

At the start of every session and after every compaction:
1. CLAUDE.md is already in context — no explicit read needed
2. Read `docs/work/now.md`
3. Read active milestone's `session.md` if it exists
4. **Stop. Do not read more files to orient. Read other files only when an action requires them.**
5. User's opening message directs what to do next — follow that

Session-start cost target: under 10K tokens before doing any actual work.

The spray-read antipattern: reading spec.md, plan.md, conventions.md, and source files "to orient" at session start. This is what causes the 70-80K token burns. session.md should be self-sufficient. If it isn't, the problem is in session.md quality, not fixable by reading more files.

---

## What Still Needs to Be Planned

Before building anything, these skills/files still need detailed planning:

- **/write-spec** — only roughly described. Needs: exact steps, how it uses brainstorm.md, what it does when brainstorm.md doesn't exist, output format validation.
- **/write-plan** — roughly described. Needs: exact codebase-read discipline, how tasks are formatted, what to do when a file doesn't exist yet.
- **/debug** — known to be adapted diagnosing-bugs. Needs: what exactly to change (CONTEXT.md/ADR refs → session.md/KB files), whether anything else needs adapting.
- **CLAUDE.md template** — content roughly planned (≤80 lines, routing table, hard rules, skills table). Needs: exact wording, confirm content fits in 80 lines.
- **scripts** — design is locked. Content not yet written.
- **Subdirectory structure for the build** — where in the repo the draft files go during development.

---

## Reference Material

Eight repos in `temp/repos/`. Full reference files in `temp/refs/`.

| Repo | Relevance |
|---|---|
| `superpowers` | What NOT to do. Brainstorming skill analyzed — useful scope-check logic, rest is bloat. |
| `mattpocock-skills` | HIGH. `grilling` is the backbone of /brainstorm. `diagnosing-bugs` is adopted. `git-guardrails-claude-code` for hook. |
| `agent-skills` | MEDIUM. `interview-me` contributed assumption check and "want vs should want" probe. `idea-refine` contributed assumption surfacing. |
| `gstack` | LOW. Too heavy. "No fixes without investigation" principle worth keeping. |
| `OpenSpec` | MEDIUM. Folder-per-change concept, "no rigid phase gates" philosophy. |
| `taste-skill` | UNKNOWN. Try on real UI task. Examples look good. |
| `ui-ux-pro-max-skill` | UNKNOWN. Examples look visually impressive. Python dependency concern. |
| `antrophic-skills` | Reference for skill format and description field behavior. |

Delapse reference in `temp/local-refs/delapse/` — m07d-eval-harness is best milestone example. Initial spec in `temp/local-refs/delapse/initial-spec/` shows what project-level design phase outputs look like.

---

## Build Order

**Phase 1 — Finish planning (do before writing any files):**
1. Plan /write-spec in detail
2. Plan /write-plan in detail
3. Plan /debug adaptation
4. Confirm CLAUDE.md content (line count, exact wording)
5. Decide subdirectory structure for draft files

**Phase 2 — Build in subdirectory:**
6. Write CLAUDE.md template
7. Write now.md template
8. Write backlog.md template
9. Write /brainstorm skill
10. Write /checkpoint skill
11. Write /write-spec skill
12. Write /write-plan skill
13. Write /debug skill (adapted diagnosing-bugs)
14. Write scripts/tree.sh
15. Write scripts/merge.sh

**Phase 3 — Review and move:**
16. Review all draft files
17. Approve
18. Move to live locations
19. Decide packaging (Decision 25)
