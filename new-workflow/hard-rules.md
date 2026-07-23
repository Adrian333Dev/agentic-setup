# Hard rules (new-workflow)

Staging area for always-active rules that will go into the new-workflow's CLAUDE.md once it's written. These are NOT skill-invocable — they apply all the time, in every phase, and must live in CLAUDE.md (always loaded) not in separate skill files.

---

## Never state a cause without evidence

After receiving any result (command output, error, user report) — separate what is *known* from what is *inferred*. Don't write "the reason is X" or "this happens because X" unless you observed evidence for X. Every causal claim must be labeled as a hypothesis and paired with a verification step: "Hypothesis: X. To verify: Y."

**Most critical during:** debugging. **Applies everywhere.**

---

## Never run git mutations

Suggest commands; the user runs them.

---

## No placeholders

Real file paths, real code, real commands — always. No `<!-- fill in later -->`, no `TODO`, no `...`.

---

## Surface reasoning before any workflow document write

Before writing to any workflow document (brainstorm.md, spec.md, plan.md, session.md, or similar) — write out in your response what you are about to write, why you decided that, and any inference beyond what was explicitly discussed in the conversation.

**When the decision is already locked** (user-confirmed, no open threads on it), record it in the same turn — no separate yes/no approval round-trip, and batch several locked items into one write rather than editing every turn. **Wait for approval only** when the write rests on an inference or judgment beyond what was explicitly discussed.

**Exception:** Minor mechanical updates that require no judgment — marking a branch `[x]` in a tree, appending an item to Deferred when already agreed — can be done inline with a brief note.

**Applies everywhere:** brainstorming, spec writing, planning, and any phase that produces workflow documents.

> The full communication ruleset (the `/copy` discipline, voice-to-text, no-fluff, explain-from-zero, no-auto-memory) now lives canonically in `agentic-workflow_v2/CLAUDE.md` — see its **Working with the user** + **Communication** sections and the **Hard rules** additions. Kept there (the shipped artifact) rather than duplicated here to avoid drift.
