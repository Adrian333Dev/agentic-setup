# Research Evaluation Log

Append-only. One entry per research session.

---

## 2026-07-09 — Cloud TTS API Pricing Comparison

**Domain tags:** tts, api-pricing, chrome-extension
**Summary:** Compared ElevenLabs, OpenAI TTS, Azure AI Speech, Google Cloud TTS, and AWS Polly on pricing, native word timestamps, and streaming text input. Key finding: only three providers have native word timestamps (AWS Polly, Azure, ElevenLabs); only ElevenLabs accepts streaming text input. At meaningful scale, all cloud options are more expensive than zero-cost local options.
**Prompts:** See `/home/me/code/projects/agentic-setup/temp/research/tts.md` (first half)

### ChatGPT (web — GPT-5.5, web search) — Cloud TTS pricing + features

**Baseline:**
- Accuracy: 4/5
- Critical coverage: 4/5
- Completeness: 5/5
- Depth: 4/5
- Hallucination risk: low

**Domain-specific:**
- Source recency: 5/5 — cited official pricing pages with URLs; API prices appear current
- Cost modeling: 5/5 — included a practical example (10k cards/month → 12.5M chars/month) with per-provider estimates; immediately usable
- Feature matrix accuracy: 5/5 — streaming text input vs. streaming audio output distinction correctly identified and called out; subtle distinction many responses conflate
- Provider coverage: 5/5 — covered all five providers with consistent structure
- Subscription pricing accuracy: 3/5 — minor misinformation on subscription plan details (not API pricing); didn't affect the decision since we were evaluating API pricing, but indicates some details weren't fully verified

**Verdict:** Strong response for API pricing comparison. Cost modeling and feature matrix were directly applicable. Minor inaccuracy on subscription plan specifics (not the focus of the research, so low impact). API pricing and feature flags were accurate. Would have been safe to act on for the API-level decision.

---

## 2026-07-09 — Open-Source / Local TTS with Word Timestamps

**Domain tags:** tts, open-source, chrome-extension, browser-inference, onnx
**Summary:** Surveyed open-source TTS options runnable client-side or on-device with word-level timestamps for a Chrome MV3 extension. Kokoro emerged as the clear winner — lightweight (82M params, ~85MB Q8), browser-capable via ONNX/WASM, and has a timestamp-enabled ONNX build (`Kokoro-82M-v1.0-ONNX-timestamped`). HeadTTS project validated that phoneme timestamps are accessible in a browser context.
**Prompts:** See `/home/me/code/projects/agentic-setup/temp/research/tts.md` (second half)

### ChatGPT (web — GPT-5.5, web search) — Open-source local TTS survey

**Baseline:**
- Accuracy: 5/5
- Critical coverage: 3/5
- Completeness: 5/5
- Depth: 4/5
- Hallucination risk: low

**Domain-specific:**
- Timestamp discovery (autonomous): 2/5 — initial response correctly identified that Kokoro computes timestamps internally but doesn't expose them via the standard JS API, and suggested workarounds; it did NOT autonomously find the `Kokoro-82M-v1.0-ONNX-timestamped` build. That artifact was found only after user provided a hint about that specific version.
- Timestamp discovery (with prompting): 5/5 — once the specific build was pointed out, the response accurately described it and validated its use
- Browser feasibility assessment: 5/5 — correctly differentiated browser vs. server vs. local deployment; Kokoro ONNX + Transformers.js path clearly identified
- Hardware requirements accuracy: 5/5 — CPU vs. GPU requirements accurate per model
- Recency: 4/5 — newer heavy models (Chatterbox, Fish Speech) correctly positioned as impractical

**Verdict:** Good survey coverage and accurate information, but the critical artifact discovery (`Kokoro-82M-v1.0-ONNX-timestamped`) required user prompting — it was not found autonomously. The initial assessment ("timestamps exist internally but aren't exposed") was partially correct but incomplete. The final accurate picture only emerged after a user hint. For research where finding a specific community artifact is the key deliverable, this is a meaningful gap. Coverage and accuracy on everything else was solid.

---

## 2026-07-09 — Chrome MV3 Extension ONNX Architecture (Prompt 1)

**Domain tags:** chrome-extension, mv3, onnx, browser-inference, architecture
**Summary:** Investigated where ONNX Runtime Web inference should run in a Chrome MV3 extension to keep a large (~85MB) model warm in memory between calls. Answer: offscreen document with reason `WORKERS` (not `AUDIO_PLAYBACK`), spawning a Dedicated Worker that holds the ONNX session. Service worker cannot run ONNX at all due to WASM backend initialization failure. Content script workers tied to tab lifecycle and page origin.
**Prompts:** See `/home/me/code/projects/agentic-setup/temp/research/architecture.md`

### ChatGPT (web) — MV3 ONNX architecture (LLM 1)

**Baseline:**
- Accuracy: 3/5
- Critical coverage: 2/5
- Completeness: 4/5
- Depth: 3/5
- Hallucination risk: medium

**Domain-specific:**
- Production gotcha detection: 2/5 — missed three critical implementation gotchas: (1) WASM/dynamic import() is banned in ServiceWorkerGlobalScope — hard error, not just lifecycle issue; (2) AUDIO_PLAYBACK reason has a 30-second idle timeout — would have caused model unload between plays; (3) CSP requires `wasm-unsafe-eval` — without it WASM initialization fails silently
- Implementation specificity: 3/5 — correct patterns at architectural level but no manifest.json snippets, no CSP directives, no WASM path configuration details
- Lifecycle accuracy: 3/5 — correctly stated offscreen documents don't have the service worker 30s timer, but didn't differentiate between reasons or mention AUDIO_PLAYBACK timeout
- Source quality: 3/5 — referenced Chrome docs and HuggingFace but not the most relevant specific pages

**Verdict:** Got the high-level direction right (offscreen > service worker > content script) and the message routing pattern was correct. However, the three missed gotchas would have caused real implementation failures: the WASM ban in service workers would have produced a cryptic error; the AUDIO_PLAYBACK timeout would have caused intermittent model loss between plays; missing the CSP requirement would have been a mystery build failure. Would need significant verification before acting on this response for implementation.

---

## 2026-07-09 — Chrome MV3 Extension ONNX Architecture (Prompt 2)

**Domain tags:** chrome-extension, mv3, onnx, browser-inference, architecture
**Summary:** Same prompt as above, second model. This response caught all three critical gotchas the first missed, provided manifest.json configuration, named specific existing Kokoro Chrome extensions, and included risk analysis for WebGPU context loss and future offscreen lifetime restrictions.
**Prompts:** See `/home/me/code/projects/agentic-setup/temp/research/architecture.md`

### Claude (version unknown) — MV3 ONNX architecture (LLM 2)

**Baseline:**
- Accuracy: 5/5
- Critical coverage: 5/5
- Completeness: 5/5
- Depth: 5/5
- Hallucination risk: low

**Domain-specific:**
- Production gotcha detection: 5/5 — caught all three critical issues: WASM/import() hard ban in service workers, AUDIO_PLAYBACK 30s idle timeout, CSP `wasm-unsafe-eval` requirement. Also surfaced: WebGPU context loss risk, single offscreen document constraint, extension update teardown behavior, future lifetime restriction risk
- Implementation specificity: 5/5 — provided manifest.json structure, exact API call patterns (`chrome.runtime.getContexts()`, `chrome.offscreen.createDocument()`), WASM path configuration code snippet, multi-threading setup
- Existing pattern discovery: 5/5 — named specific real extensions (chaimantec's Kokoro Chrome extension, Kokoro TTS Engine at webextension.org) confirming the architecture works in production
- Risk analysis: 5/5 — proactively called out future Chrome lifetime restriction risk for offscreen documents and recommended defensive coding patterns
- Source quality: 5/5 — cited specific Chrome developer docs pages and Chromium proposals, not just high-level docs

**Verdict:** Reference-quality response. The WASM/import() ban alone would have saved hours of debugging — it's not mentioned in most architecture guides. The AUDIO_PLAYBACK timeout is a subtle gotcha that would have caused intermittent failures that are hard to reproduce. WebGPU context loss risk was unprompted but directly relevant. This response drove final architectural decisions with zero need for verification on critical claims. Clear winner for browser extension architecture questions.

---

## 2026-07-18 — Claude Code Tooling Ecosystem (MCP / plugins / skills)

**Domain tags:** ai-tooling, mcp, claude-code, agent-skills, ecosystem-research
**Summary:** Surveyed MCP servers, Claude Code plugins, and standalone agent skills (mid-2026) to enrich the template's `recommended-tools.md`, excluding anything that duplicates the in-house workflow. Shared prompt run across four models. Convergent finding: the highest-value additions are capability/integration MCPs + narrow guardrail plugins, NOT more orchestration; and several once-standard tools (official Postgres MCP, Puppeteer MCP) are now archived/unmaintained and must be skipped. Claude alone surfaced the decisive Claude-Code-specific gotcha — GitHub MCP is a net negative here because the `gh` CLI is ~30× cheaper in tokens. Ranking: **Claude > ChatGPT > DeepSeek > Gemini.**
**Prompts:** Single shared prompt, 4 models. Report at `/home/me/code/projects/agentic-setup/temp/research/workflow-enrichment.md`; prompt at `scratchpad/recommended-tools-research-prompt.md`.
**Autonomy:** No mid-research hints given to any model — all ran independently off one prompt. No autonomy penalty.

### ChatGPT (version as labeled; variant unconfirmed) — Tooling ecosystem survey

**Baseline:**
- Accuracy: 4/5
- Critical coverage: 4/5
- Completeness: 5/5
- Depth: 4/5
- Hallucination risk: low-medium

**Domain-specific:**
- Deprecation/maintenance accuracy: 3/5 — did not flag the archived Postgres/Puppeteer reference MCPs that others caught
- Redundancy discipline: 5/5 — explicit exclusion list up front + per-entry overlap caveats; best-articulated framing of "does this duplicate the in-house layer?"
- Install-command accuracy: 3/5 — several entries hand-wave ("install per its current README") instead of a runnable command
- Signal-to-noise / targeting: 4/5 — sensible compact "core + add-when-relevant" baseline; flagged Sequential-Thinking MCP as skip
- Security awareness: 5/5 — strongest on skill provenance/lifecycle security; cited a SWE-skills benchmark arguing most skills add no value (good selectivity argument)

**Verdict:** Excellent structure, completeness, and the sharpest security/selectivity framing of the four. But it missed two concrete gotchas Claude caught — the archived-and-vulnerable Postgres MCP and the GitHub-MCP-vs-`gh` cost problem — and leaned on vague install instructions. Safe to act on directionally; verify install commands. The one unverifiable arXiv citation is a minor hallucination-risk flag.

### Gemini (version unknown) — Tooling ecosystem survey

**Baseline:**
- Accuracy: 3/5
- Critical coverage: 2/5
- Completeness: 4/5
- Depth: 3/5
- Hallucination risk: medium-high

**Domain-specific:**
- Deprecation/maintenance accuracy: 2/5 — no archived-tool flags; recommended likely-wrong npm package names as if current
- Redundancy discipline: 3/5 — respected the exclusion list but over-applied it, wrongly dropping genuinely useful tools (Playwright, Sentry) by conflating a browser-automation *capability* with the debugging *methodology* the template owns
- Install-command accuracy: 2/5 — dubious `npx @figma/mcp-server` / `@stripe/mcp-server` / `@vercel/mcp-server` names; Figma's official is a remote server, not that package
- Signal-to-noise / targeting: 3/5 — some niche picks (n8n, Slack, Docker) with thin justification
- Security awareness: 2/5 — minimal

**Verdict:** Weakest of the four. Riddled with broken `[cite: x.x.x]` citation artifacts (unverifiable, suggests it didn't actually verify), several install commands that would error if run, and a category-confusion that made it discard useful tools. Would need fact-checking on nearly every install line before acting. Did correctly surface the TypeScript LSP plugin and Semgrep, which the others under-weighted.

### DeepSeek Instant — Tooling ecosystem survey

**Baseline:**
- Accuracy: 3/5
- Critical coverage: 3/5
- Completeness: 4/5
- Depth: 3/5
- Hallucination risk: medium

**Domain-specific:**
- Deprecation/maintenance accuracy: 4/5 — the session's single best specific catch: flagged the official Postgres MCP as archived AND carrying a known SQL-injection flaw that bypassed its read-only guarantee. But recommended the outdated `@modelcontextprotocol/server-playwright` package (superseded by `@playwright/mcp`)
- Redundancy discipline: 4/5 — respected exclusions cleanly
- Install-command accuracy: 3/5 — good concrete remote-OAuth endpoints for most MCPs; one clearly outdated Playwright command
- Signal-to-noise / targeting: 2/5 — the skills section is low-signal: dumps generic collections (justjavac, shengyy, @skill-hub) and an irrelevant Metaplex/Solana skill for a TS/React dev — the exact "grab-bag" pattern the task warned against
- Security awareness: 3/5 — strong on the Postgres vuln, light elsewhere

**Verdict:** Usable for its MCP list and quick-reference table, and it produced the sharpest single security catch of the session (archived + injectable Postgres MCP). But the skills section is noise and one install command is stale. Take the MCP picks, ignore the standalone-skills recommendations.

### Claude (version as labeled; variant unconfirmed) — Tooling ecosystem survey

**Baseline:**
- Accuracy: 5/5
- Critical coverage: 5/5
- Completeness: 5/5
- Depth: 5/5
- Hallucination risk: low

**Domain-specific:**
- Deprecation/maintenance accuracy: 5/5 — flagged every relevant deprecation (Postgres MCP, Puppeteer MCP, `server-puppeteer`, old `semgrep/mcp` in favor of Guardian)
- Redundancy discipline: 5/5 — the only response with an explicit "skip — duplicates your template" section (Superpowers, Claude Mem, planning-with-files, ECC harnesses), directly serving the actual ask
- Install-command accuracy: 5/5 — correct current packages (`@playwright/mcp@latest`, official marketplace ids)
- Signal-to-noise / targeting: 5/5 — tight, relevant set; distinguished overlapping tools (DevTools MCP vs Playwright MCP; code-review plugin vs completion-verification skill). Mild over-tailoring to the specific project (Delapse/NestJS) — needs generalizing for a template
- Security awareness: 4/5 — strong deprecation/vuln flags; slightly less explicit than ChatGPT on the "review provenance before install" meta-point

**Verdict:** Best response by a clear margin and directly actionable. The GitHub-MCP-vs-`gh` token-cost gotcha (~30× cheaper via CLI) and the explicit duplicates-your-template skip list are exactly what the task needed and no other model produced either. The only cleanup needed is generalizing its project-specific tailoring; unverifiable star/install counts are the sole minor risk. Would have driven the `recommended-tools.md` rewrite with near-zero verification.
