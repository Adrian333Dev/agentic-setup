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
