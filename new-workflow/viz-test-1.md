# Visualization Skill — Battle Test Log

---

## Case 1 — Step 0 gate: "Explain what a Promise is"

**What was tested:** Does the skill correctly avoid producing a diagram when text suffices?

**Gate result:** Correct — no diagram produced, text format used.

---

### Finding 1: Telegraph style is wrong for explanatory content

**What went wrong:**  
`text-formats.md` instructs "telegraph style throughout — fragments over sentences." This produces dense, jargon-heavy output when the Step 0 gate fires on *explain X* requests. The output feels like minified docs, not an explanation.

**Fix applied to `text-formats.md`:**  
Removed the blanket "telegraph style" rule. Replaced with: plain sentences for explanatory text; short labels inside tree/flow blocks only.

**Rule going forward:**  
- Inside a text-tree or sequential flow block → one line per step, no filler, labels stay short  
- Anywhere that reads like a definition, lead-in, or rule callout → plain readable sentence

---

### Finding 2: Skill assumes user already knows the domain

**What went wrong:**  
Explanations (and by extension, diagram annotations and proposals) assume the user is familiar with the environment or concept being shown. This breaks down whenever the user is working outside their expertise — e.g., an agent explaining a browser extension's three-environment architecture to someone who has never built one. The agent skips the "what this even is" layer and jumps straight to the detail.

**Why this matters especially for visualization:**  
The whole point of a visual is to make something clear. If the labels, annotations, and surrounding prose assume prior knowledge, the diagram fails its job even if it renders perfectly.

**Fix to add to the skill:**  
Add a rule under the output workflow: when producing any visual or text-format explanation, do not assume familiarity with the environment or implementation pattern being shown. Include one plain-language sentence establishing what the thing *is* before describing how it works. Labels inside diagrams should be self-evident to someone seeing the concept for the first time.

---


## Case 2 — Real codebase explanation: Delapse extension architecture

**What was tested:** Can the skill explain an unfamiliar codebase (a Chrome extension) to someone who has never seen extension code and doesn't know how browser extensions work?

**Initial result:** First attempt produced a single dense structural diagram cataloguing what code lives where — useful as a reference but completely opaque to someone unfamiliar with the domain. Jargon like "Content Script", "Main World", "SPA nav" appeared without definition.

---

### Finding 3: Plan before producing — what to visualize and in what order

**What went wrong:**  
The skill jumped straight to generating a diagram without first thinking through what the user actually needs to understand, in what order. A structural diagram showing what lives in each environment is useless if the user doesn't yet know what "environment" means in the context of a browser extension.

**The fix — a loose planning step before any diagram:**  
When the topic has layers (what something IS, how it's structured, how data flows through it), pause before generating anything and briefly answer:
- What does the user not know yet?
- What do they need to understand first before the next piece will make sense?
- Is this one diagram, or a sequence of text + diagrams?

This planning step should be lightweight — a few sentences, not a formal spec. The goal is simply to avoid producing a diagram before understanding what mental model is being built.

**If triggered from brainstorming:**  
The conversation context already contains signal about which concepts are being introduced for the first time. Use it — don't re-derive from scratch. The plan will usually be shorter because some context is already established.

**Pattern that works:**  
Plain text establishing the concept → diagram showing the structure or flow → plain text bridging to the next idea → next diagram. Never a single large diagram trying to cover everything at once.

---

### Finding 4: SVG text placement — avoiding overlap in tight spaces

**What went wrong:**  
Two recurring collision patterns appeared in the SVG:

1. A long centered label on a boundary line (the security boundary annotation) placed at almost the same y-position as a nearby status label ("✕ blocked here") — they overlapped because both were in the same narrow band between two containers.

2. A floating "outside the browser" text label and a "fetch localhost" arrow label were both placed in a 28px gap, with the arrow also passing through — three elements sharing the same band.

**Rules to prevent this:**

- **Labels near arrows or boundary lines go to the SIDE**, not at the same y-level as other elements in the same band. Offset horizontally from the line; don't just float at a nearby y-value.
- **Long centered labels in narrow gaps always collide.** Fix by shortening the label, switching to left-aligned placement, or using the "badge on a line" pattern.
- **Badge on a line pattern:** draw a small background rect over the line at the label position, then place the text on top. The rect masks the line behind the label so it reads as an annotated rule, not overlapping elements. Works cleanly for boundary/separator lines.
- **Before placing any label in a gap area** (the space between two containers), check: does anything else share a similar y-range in the same x-range? Arrow + label + boundary text cannot all coexist in the same 30–40px band without explicit x-separation.
- **Floating text outside boxes** (labels like "outside the browser", "external", etc.) will almost always collide with nearby arrows. Move the information inside the relevant box as a subtitle instead.

---

## Cases pending

| # | Scenario | Type | Status |
|---|---|---|---|
| 1 | "Explain what a Promise is" | Step 0 gate | ✅ Done |
| 2 | "Walk me through the OAuth2 authorization code flow" | Flowchart | Pending |
| 3 | "Architecture of a Next.js app with DB + auth" | Structural | Pending |
| 4 | "Compare REST vs GraphQL vs tRPC" | Comparison | Pending |
| 5 | "Give me intuition for how the JS event loop works" | Illustrative | Pending |
| 6 | "Schema for a SaaS app: users, orgs, subscriptions" | ERD (DBML) | Pending |
| 7 | "Mockup a task management dashboard sidebar" | UI mockup | Pending |
| 8 | "Microservices system: auth, payments, gateway, 3 domain services..." | Complex diagram | Pending |
| 9 | "Show me how React renders AND the component tree" | Mixed intent | Pending |
