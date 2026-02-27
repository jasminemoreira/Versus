# Versus for Copilot

**IACDM (Iterative Adversarial Convergence Development Methodology) orchestrator for GitHub Copilot Agent Mode.**

An original methodology created by [Jasmine Moreira](https://github.com/jasminemoreira) that transforms LLMs from reactive code generators into disciplined agents following a structured 8-phase process — from problem discovery to delivery with tests and post-review.

## Why?

AI coding tools (Lovable, Cursor, Claude Code, ChatGPT) all share the same fundamental limitation: **zero internal verification capability**. They generate statistically plausible output without distinguishing correct code from incorrect code. The METR study (2025) showed experienced developers using AI tools were actually **19% slower** — despite believing they were 20% faster.

**The real distinction isn't between tools — it's between process and no process.**

Versus implements the AG/AV (Generative Agent / Verification Agent) model: the AI generates, external agents (automated tests + human operator) verify at discrete **gates**. Errors are caught at the earliest possible phase, not in production.

## Features

- **8 sequential phases** with verifiable exit criteria — no skipping steps
- **8 safeguards (S0-S7)** that prevent common AI agent errors (premature convergence, scope creep, reimplementation)
- **Convergence score** (0-100) that gates advancement until the problem is truly understood
- **15 MCP tools** for state management, decision recording, and phase transitions
- **Persistent decision registry** that survives context switches
- **Gateway guard** that detects when models skip loading context
- **Adversarial critique** with 7 universal + 8 conditional specialized lenses
- **VS Code sidebar** showing methodology state in real time
- **Multi-session testing protocol** for Phase 6 continuity across sessions
- **HLI (Human Lead Index)** — anti-vibe-coding metric (0-10) that measures human leadership across phases, with per-phase breakdown and drop alerts
- **Meta-iteration** (v1→v2): `start_new_cycle()` resets to Phase 0 preserving all decisions, specs/, and history

## How It Works

```
Phase 0: Problem Discovery    → HSA 5-level exploration (score >= 90 to advance)
Phase 1: Architecture         → Define modules, interfaces, patterns
Phase 2: Adversarial Critique → Attack the architecture with specialized lenses
Phase 3: Simplification       → Address criticals, simplify (loops with Phase 2)
Phase 4: Convergence Gate     → Validate all exit criteria + safeguards
Phase 5: Implementation       → Code the final architecture
Phase 6: Tests                → 100% passing + exploratory testing
Phase 7: Post-Review          → Double-loop learning: evaluate product AND process
```

### Phase 0: Hierarchical Semantic Analysis (HSA)

The most distinctive contribution. Structures problem exploration in 5 levels, each building on the previous:

| Level | Focus | What is sought |
|---|---|---|
| 1. Domain | Universe where the problem exists | Vocabulary, theoretical/technical field, tools, state of the art |
| 2. Problem | What needs to be solved | 5W1H: who, what, when, where, why, how solved today |
| 3. Elements | Parts composing the problem | Components, entities, actors, constraints |
| 4. Processes | How parts relate | Flows, transformations, dependencies, feedback cycles |
| 5. Product | Expected outcome | Deliverables, acceptance criteria, success metrics |

### Context Efficiency

The methodology maximizes `E = I₀/C` (relevant information / total context consumed) through granularization: each session focuses on one module with just its interface signatures, keeping context efficiency near 1.0 instead of degrading to 0.3+ in monolithic sessions.

## Quick Start

1. Install the extension
2. Open the Command Palette (`Ctrl+Shift+P`) and run **Versus Copilot: Initialize Project**
3. Enter a project name and description when prompted
4. Open a **new Copilot Agent Mode conversation**
5. Type **start** — the agent loads methodology state and begins Phase 0

> **Important:** After initializing, always start a new conversation so Copilot picks up the MCP server and hooks. If the agent doesn't use MCP tools, run `Developer: Reload Window` from the Command Palette.

## The 8 Safeguards

| ID | Name | Protects Against |
|---|---|---|
| S0 | Problem Convergence | Advancing without understanding the problem |
| S1 | Anti-Bug | Simplification that introduces bugs |
| S2 | Stopping Criterion | AI deciding when to stop (user's decision) |
| S3 | Premature Convergence | Stopping iteration too early |
| S4 | Explicit Verification | Skipping human validation at gates |
| S5 | Scope Preservation | Scope creep during critique-simplification |
| S6 | Do Not Reimplement | Recreating what already exists |
| S7 | Sequence Discipline | Starting tangential discussions during implementation |

## The AG/AV Model

```
F₀ → G₀ → F₁ → G₁ → ... → Fₙ → Gₙ → delivery
      ↑              ↑                    ↑
  AV evaluates   AV evaluates        AV evaluates
```

- **AG (Generative Agent):** The LLM. Produces artifacts without verification capability.
- **AV-automatic:** Tests, linters, compilers — binary verdict on formalizable properties.
- **AV-human:** The operator — evaluates semantic adequacy, usability, domain correctness.
- **Gate (Gₖ):** Discrete point where progression requires AV approval. Rejection feeds concrete error information back to AG.

## Requirements

| Requirement | Minimum | Why |
|---|---|---|
| VS Code | >= 1.99.0 | Extension API compatibility |
| Node.js | >= 18.0.0 | Runs the MCP server (`node .versus/server.js`) |
| GitHub Copilot | Latest | Agent Mode for MCP tools and hooks |

> **Warning:** Node.js is mandatory. Without it, the MCP server cannot start and the agent will ignore the methodology entirely. Install from [nodejs.org](https://nodejs.org).

## LLM Compatibility

The IACDM methodology demands **strict instruction following**, **reliable MCP tool calling**, and **sustained reasoning across long contexts**. Not all models meet these requirements equally.

### Selection Criteria

| Criterion | Why it matters |
|---|---|
| **Instruction following** | Each phase has detailed behavioral rules (~2-4k tokens). Models that summarize or skip steps break the methodology |
| **MCP tool reliability** | The methodology relies on 15 tools being called consistently. Models that "narrate" tool calls instead of executing them are incompatible |
| **Context window** | Phases 0-2 accumulate significant context. Minimum 32k tokens, recommended 128k+ |
| **Reasoning depth** | Phases 0-4 (design) require architectural analysis, not code generation. Shallow reasoning produces shallow designs |

### Model Tier Classification

| Tier | Models | Phases 0-4 (Design) | Phases 5-7 (Code) | Notes |
|---|---|---|---|---|
| **S** | Claude Opus, o3 | Excellent | Excellent | Best instruction following. Highest cost |
| **A** | Claude Sonnet, GPT-4o | Very good | Very good | Best cost/quality balance. Occasionally simplify Phase 2 critique |
| **B** | GPT-4.1, Gemini 2.5 Pro | Good with caveats | Good | May misinterpret instructions as user questions. Require Gateway Guard |
| **C** | Mini/Flash/Lite models | Not recommended | Acceptable | Skip safeguards, simplify critique, ignore R5. Only for Phases 5-7 with validated architecture |

### Recommended Strategy (LLM Switch Point)

The methodology includes a natural **LLM Switch Point at Phase 4**. The architecture is fully validated and persisted in `state.json` + `specs/`, so no context is lost when switching models.

| Strategy | Phases 0-4 | Phases 5-7 | Best for |
|---|---|---|---|
| **Maximum quality** | Tier S | Tier S | Critical/high-complexity projects |
| **Optimal cost/quality** | Tier S | Tier A/B | Most projects — leverages the LLM Switch Point |
| **Budget** | Tier A | Tier A/B | Medium-complexity projects |

> **Rule of thumb:** The cost of architectural error (detected in Phase 5-6) is 10-100x the cost of using a more capable model in Phases 0-4. Invest in reasoning where reasoning matters.

### Built-in Protections for Weaker Models

| Mechanism | Protects against |
|---|---|
| **Gateway Guard** | Model forgetting to load context (>30min without `get_phase_state`) |
| **Phase Gate hook** | Code editing in Phases 0-4 (blocked regardless of model) |
| **Loop Detector** | Repetitive patterns (>3x same action) — safeguard S7 |
| **Compact Guidance** | Summarized instructions injected into every prompt via hook |
| **Phase 7 Engine Hint** | Forces meta-iteration offer even if model didn't read guidance |

## HLI (Human Lead Index)

An anti-vibe-coding metric that measures how much the human is actually leading the AI-assisted development process. Calculated automatically at each phase transition.

**Formula:** `HLI(phase) = 0.3 × criteriaRatio + 0.3 × decisionDensity + 0.4 × phaseSpecific`

| Phase | Phase-Specific Indicator | Measures |
|-------|--------------------------|----------|
| 0 | Phase 0 Score | Problem understanding depth |
| 1 | Category Diversity | Breadth of architectural decisions |
| 2-3 | Iteration Depth | Adversarial critique engagement |
| 4 | Safeguard Health | Process integrity at convergence |
| 5 | Loop Efficiency | Implementation without excessive retries |
| 6 | Spec Coverage | Tests mapped to specs (not implementation) |
| 7 | Lessons Quality | Post-review completeness |

- **Composite score (0-10)** displayed in the VS Code sidebar with per-phase breakdown
- **Drop alerts** triggered when score falls > 2 points between consecutive phases
- **Injected into context** so the LLM is aware of process quality in real time

> The market measures AI coding **productivity** (lines/hour, PRs/day). HLI measures **rigor** — whether the human is leading or just accepting output.

## Companion Extension

Use **[Versus for Claude](https://marketplace.visualstudio.com/items?itemName=jasminemoreira.versus-claude)** for Claude Code integration. Both extensions share the same `.versus/state.json` format.

## Changelog

### v0.3.3 — 2026-02-27

**Added:**
- **HLI (Human Lead Index):** Anti-vibe-coding metric (0-10) calculated at each phase transition. Three components: criteria ratio (30%), decision density (30%), phase-specific indicator (40%). Per-phase breakdown in sidebar, drop alerts (>2 points), injected into context hook and MCP tool responses

### v0.3.2 — 2026-02-27

**Added:**
- **Spec-Driven Test Protocol (Phase 6):** Tests must map to specs, not implementation. Test Map (MUST DISPLAY), mandatory negative tests (1:2 ratio), distinction between "test green" vs "spec met", spec coverage report as traceable decision
- **LLM Compatibility section** in README with model tiers (Opus S / Sonnet A / GPT-4.1 B / Mini C), LLM Switch Point strategy, and built-in protections table
- **Phase 3 post-cycle decision rewrite:** AI computes structural change % and criticals, presents recommendation, user confirms (instead of asking user to choose percentage range)
- **Phase 5 autonomous implementation:** "Do NOT ask permission between files" — implement sequentially, stop only for blockers

**Changed:** Publisher ID to `JasmineMoreira`. Repository/homepage URLs point to public repo.

### v0.3.1 — 2026-02-27

**Added:**
- **Phase 7 engine-level hints:** `_phase7Action` in `get_phase_state` and `mark_exit_criterion` responses forces meta-iteration offer regardless of guidance reading
- **Delivery Target question (Phase 0 Level 5):** Mandatory question — Complete product / MVP / Prototype. Conditions Phase 1 Progressive Scope
- **R5 Mandatory Display:** Global rule forcing display of essential outputs with MUST DISPLAY markers across all phases
- **Node.js requirement:** Added to README Quick Start and package.json engines

**Changed:** Phase 1 Progressive Scope conditioned on delivery target. Phase 5 S7 reinforced for autonomous implementation.

**Fixed:** Phase 7 end-of-cycle — `isValidTransition(7, 0)` returned false and `initProject()` wiped context. New dedicated `startNewCycle()` method solves both.

### v0.3.0 — 2026-02-26

Initial stable release: 15 MCP tools, 3 Copilot hooks (inject-context, phase-gate, loop-detector), Gateway Guard, VS Code sidebar, multi-session testing protocol.

## License

[MIT](LICENSE) - Created by [Jasmine Moreira](https://github.com/jasminemoreira).
