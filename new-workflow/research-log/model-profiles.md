# Model Profiles

Running performance profiles built from evaluation log. Updated after each research session.

Last updated: 2026-07-18
Total sessions evaluated: 4 (8 individual responses)

> **Version caveat (2026-07-18 session):** the four responses were labeled only by family (ChatGPT, Gemini, DeepSeek Instant, Claude). Exact variants for this session are unconfirmed — ChatGPT/Claude entries are merged into their existing profiles on the assumption they match the prior variants. Confirm variants if profile precision matters.

---

## ChatGPT (web — GPT-5.5 (free plan) Web Search enabled)

Sessions: 4 | Responses: 4
Last evaluated: 2026-07-18

<!-- Raw scores for recalculation:
Session 1 (Cloud TTS pricing): Acc 4, Crit 4, Comp 5, Depth 4 | Halluc: low
Session 2 (Open-source TTS): Acc 5, Crit 3, Comp 5, Depth 4 | Halluc: low
Session 3 (MV3 architecture): Acc 3, Crit 2, Comp 4, Depth 3 | Halluc: medium
Session 4 (AI tooling ecosystem): Acc 4, Crit 4, Comp 5, Depth 4 | Halluc: low-medium
-->

**Baseline averages:**
- Accuracy: 4.0
- Critical coverage: 3.3
- Completeness: 4.8
- Depth: 3.8
- Hallucination: low (3 sessions), medium (1 session)

**Strengths:**
- Comparative/survey research — excellent at structured comparison across many options with consistent criteria (TTS pricing table, tooling ecosystem survey both immediately usable)
- Completeness — consistently covers everything requested with structured, readable output; strongest "does this duplicate my existing layer?" framing in the tooling session
- Security / selectivity framing — best of the field at arguing for restraint (skill-provenance risk, benchmark-backed "most skills add no value")
- Cost modeling — practical dollar estimates and usage projections
- Accuracy on established facts — pricing, feature flags, hardware requirements

**Weaknesses:**
- Critical-gotcha detection — repeatedly gets high-level direction right but misses the one decisive detail: 3 Chrome MV3/ONNX init gotchas (S3); the archived Postgres MCP and the GitHub-MCP-vs-`gh` cost problem (S4)
- Autonomous artifact discovery — needed a user hint to locate the specific `Kokoro-82M-v1.0-ONNX-timestamped` build (S2)
- Deep implementation specifics — stays architectural; vague install commands in the tooling survey ("install per README")
- Minor unverifiable citations (an arXiv paper in S4)

**Pattern notes:**
- Strong at breadth, structure, and comparison; weaker at finding the single critical artifact/gotcha/deprecation
- Recommended for: pricing/feature comparisons, library surveys, structured option matrices, "what should I exclude" framing
- Use caution for: production architecture for complex APIs; catching deprecations/cost traps; finding obscure-but-critical artifacts without hints; verify install commands and key technical claims independently

| Domain | Accuracy | Crit. coverage | Completeness | Depth | Sessions |
|---|---|---|---|---|---|
| api-pricing | 4.0 | 4.0 | 5.0 | 4.0 | 1 |
| open-source / onnx | 5.0 | 3.0 | 5.0 | 4.0 | 1 |
| chrome-extension / mv3 | 3.0 | 2.0 | 4.0 | 3.0 | 1 |
| ai-tooling / mcp | 4.0 | 4.0 | 5.0 | 4.0 | 1 |

---

## Claude (web - Sonnet 4.6 (free plan) Web Search enabled)

Sessions: 2 | Responses: 2
Last evaluated: 2026-07-18

<!-- Raw scores for recalculation:
Session 1 (MV3 architecture): Acc 5, Crit 5, Comp 5, Depth 5 | Halluc: low
Session 2 (AI tooling ecosystem): Acc 5, Crit 5, Comp 5, Depth 5 | Halluc: low
-->

**Baseline averages:**
- Accuracy: 5.0
- Critical coverage: 5.0
- Completeness: 5.0
- Depth: 5.0
- Hallucination: low

**Strengths:**
- Critical-gotcha & deprecation detection — the standout trait across both sessions: caught every MV3/ONNX spec gotcha (S1); flagged every archived tool AND the unique GitHub-MCP-vs-`gh` token-cost trap no other model saw (S2)
- Actionability — directly usable output with correct, current install commands and code patterns; near-zero verification needed on critical claims
- Task alignment — only S4 response with an explicit "skip, duplicates your template" section; distinguishes genuinely-overlapping tools with real nuance
- Risk surfacing & source quality — proactively flags future/edge risks and cites specific pages

**Weaknesses:**
- Over-tailoring — S4 response leaned hard into the specific project context (Delapse/NestJS); great for targeting, but needs generalizing when the deliverable is a reusable template
- Unverifiable adoption numbers — star counts / install figures stated confidently (minor hallucination-risk surface)
- Still only 2 sessions — pattern is strong but not yet broad across domains

**Pattern notes:**
- Consistently best-in-class on deep architecture, deprecations, security constraints, and "what's the non-obvious trap" detection — 2/2 reference-quality
- Recommended for: complex browser/platform architecture, security constraints, tooling/deprecation vetting, production gotcha detection, anything where the critical detail matters more than breadth
- Watch for: over-specific tailoring (generalize its output for reusable artifacts); spot-check confident numeric claims

| Domain | Accuracy | Crit. coverage | Completeness | Depth | Sessions |
|---|---|---|---|---|---|
| chrome-extension / mv3 | 5.0 | 5.0 | 5.0 | 5.0 | 1 |
| ai-tooling / mcp | 5.0 | 5.0 | 5.0 | 5.0 | 1 |

---

## Gemini (version unknown)

Sessions: 1 | Responses: 1
Last evaluated: 2026-07-18

<!-- Raw scores for recalculation:
Session 1 (AI tooling ecosystem): Acc 3, Crit 2, Comp 4, Depth 3 | Halluc: medium-high
-->

**Baseline averages:**
- Accuracy: 3.0
- Critical coverage: 2.0
- Completeness: 4.0
- Depth: 3.0
- Hallucination: medium-high

**Strengths:**
- Breadth — covered all three categories with concrete-looking entries and attempted runnable install commands
- Surfaced a couple of picks the others under-weighted (TypeScript LSP plugin, Semgrep)

**Weaknesses:**
- Verification — littered with broken `[cite: x.x.x]` citation artifacts, signalling it didn't actually ground its claims
- Install-command accuracy — several dubious npm package names (`@figma/mcp-server`, `@stripe/mcp-server`, `@vercel/mcp-server`) that would likely error if run
- Judgment — over-applied the exclusion list, wrongly discarding useful tools (Playwright, Sentry) by conflating a capability with the template's methodology
- No deprecation/security awareness — missed archived tools entirely

**Pattern notes:**
- Confident but under-verified; treat every factual/install claim as unchecked
- Recommended for: nothing critical yet on this one data point; possibly idea-generation/breadth if outputs are independently verified
- Use caution for: anything where install commands or deprecation status must be right; needs a fact-checking pass before acting

| Domain | Accuracy | Crit. coverage | Completeness | Depth | Sessions |
|---|---|---|---|---|---|
| ai-tooling / mcp | 3.0 | 2.0 | 4.0 | 3.0 | 1 |

---

## DeepSeek Instant

Sessions: 1 | Responses: 1
Last evaluated: 2026-07-18

<!-- Raw scores for recalculation:
Session 1 (AI tooling ecosystem): Acc 3, Crit 3, Comp 4, Depth 3 | Halluc: medium
-->

**Baseline averages:**
- Accuracy: 3.0
- Critical coverage: 3.0
- Completeness: 4.0
- Depth: 3.0
- Hallucination: medium

**Strengths:**
- Sharpest single security catch of the S4 field — flagged the official Postgres MCP as archived AND carrying a known SQL-injection flaw that bypassed its read-only guarantee
- Usable structure — clean quick-reference table; concrete remote-OAuth endpoints for most MCP servers

**Weaknesses:**
- Skill curation — the standalone-skills section is low-signal: dumps generic collections (justjavac, shengyy, @skill-hub) plus an irrelevant Solana/Metaplex skill for a TS/React dev
- Staleness — recommended the outdated `@modelcontextprotocol/server-playwright` package (superseded by `@playwright/mcp`)
- Missed the GitHub-MCP-vs-`gh` cost trap

**Pattern notes:**
- Decent MCP-server coverage with one strong security instinct; weak at curating/targeting skills
- Recommended for: MCP-server enumeration, quick-reference tables, a security-minded second opinion on infra tools
- Use caution for: standalone-skill recommendations (grab-bag tendency); verify package/version currency

| Domain | Accuracy | Crit. coverage | Completeness | Depth | Sessions |
|---|---|---|---|---|---|
| ai-tooling / mcp | 3.0 | 3.0 | 4.0 | 3.0 | 1 |
