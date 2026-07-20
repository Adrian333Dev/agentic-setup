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

Before writing to any workflow document (brainstorm.md, spec.md, plan.md, session.md, or similar) — write out in your response what you are about to write, why you decided that, and any inference beyond what was explicitly discussed in the conversation. Then stop and wait for explicit user approval before making the change.

**Exception:** Minor mechanical updates that require no judgment — marking a branch `[x]` in a tree, appending an item to Deferred when already agreed — can be done inline with a brief note.

**Applies everywhere:** brainstorming, spec writing, planning, and any phase that produces workflow documents.
