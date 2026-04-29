# Milestones

One folder per milestone. **Never pre-define more than one milestone ahead** — the next milestone is planned only after the current one ships.

---

## Naming

`m<NN>-<feature-slug>/` — e.g. `m01-authentication/`, `m02-dashboard/`

If a milestone unexpectedly grows, split it: `m01a-auth-core/`, `m01b-auth-social/`

## Folder contents

| File | Written by | Purpose |
|------|-----------|---------|
| `spec.md` | `superpowers:brainstorming` | Feature design and requirements |
| `plan.md` | `superpowers:writing-plans` | Step-by-step implementation plan |
| `session.md` | `checkpoint` skill | Latest session snapshot for resuming |
| `progress.md` | Optional, manual | Deferred items, notes, decisions |

## Flow

```
brainstorming → spec.md
writing-plans → plan.md
implement (subagent-driven-development or executing-plans)
checkpoint → session.md (save mid-session)
verification-before-completion
requesting-code-review
finishing-a-development-branch → update now.md → next milestone
```

## Philosophy

One milestone = one feature. Small scope = fast feedback = less wasted work. If a milestone takes more than a few days, it's probably too big — split it.
