# Project Specification

Add your project documentation here **before** running `/project-init`.

The `project-init` skill reads all files in this folder and uses them to populate `CLAUDE.md`, `AGENTS.md`, and the `docs/agents/` files.

---

## Recommended files

| File | What to put in it |
|------|--------------------|
| `product.md` | Product overview, core features, user stories, V1 scope, non-goals |
| `tech.md` | Architecture decisions, tech stack rationale, system design, deployment target |
| `data-model.md` | Database schema, table relationships, RLS/auth rules, data conventions |
| `api.md` | Endpoint contracts, request/response shapes, auth flow |
| `decisions.md` | Key technical decisions and their rationale (use as an ADR log) |

You can use any file names — `project-init` reads everything here regardless of name.

---

## Tips

- Be specific about your **V1 scope** — what's in, what's explicitly out. This drives the first milestone.
- Document your **tech stack decisions** early. "We chose X because Y" prevents relitigating those decisions later.
- Keep `decisions.md` updated as the project evolves. It's the authoritative record of why things are the way they are.
