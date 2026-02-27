# Versus for Claude

**IACDM (Iterative Adversarial Convergence Development Methodology) orchestrator for Claude Code.**

An original methodology created by [Jasmine Moreira](https://github.com/jasminemoreira) that transforms LLMs from reactive code generators into disciplined agents following a structured 8-phase process — from problem discovery to delivery with tests and post-review.

## Why?

AI coding tools (Lovable, Cursor, Claude Code, ChatGPT) all share the same fundamental limitation: **zero internal verification capability**. They generate statistically plausible output without distinguishing correct code from incorrect code. The METR study (2025) showed experienced developers using AI tools were actually **19% slower** — despite believing they were 20% faster.

**The real distinction isn't between tools — it's between process and no process.**

Versus implements the AG/AV (Generative Agent / Verification Agent) model: the AI generates, external agents (automated tests + human operator) verify at discrete **gates**. Errors are caught at the earliest possible phase, not in production.

## Features

- **8 sequential phases** with verifiable exit criteria — no skipping steps
- **8 safeguards (S0-S7)** that prevent common AI agent errors (premature convergence, scope creep, reimplementation)
- **Convergence score** (0-100) that gates advancement until the problem is truly understood
- **15 MCP tools** for state management, decision recording, phase transitions, and meta-iteration (v1→v2)
- **3 Claude Code hooks** — context injection, edit blocking in early phases, loop detection
- **Persistent decision registry** that survives context switches
- **Gateway guard** that detects when models skip loading context
- **Adversarial critique** with 7 universal + 8 conditional specialized lenses
- **VS Code sidebar** showing methodology state in real time
- **Multi-session testing protocol** for Phase 6 continuity across sessions

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
2. Open the Command Palette (`Ctrl+Shift+P`) and run **Versus Claude: Initialize Project**
3. Enter a project name and description when prompted
4. Open a **new Claude Code conversation** (the extension configures hooks and MCP on initialization)
5. Type **`start`** — Claude will automatically load the methodology state and begin Phase 0

> **Important:** After initializing, always start a **new conversation** so Claude picks up the MCP server and hooks. If Claude doesn't use MCP tools, run "Developer: Reload Window" from the Command Palette.

## Claude Code Hooks

| Hook | Event | Function |
|---|---|---|
| **inject-context** | `UserPromptSubmit` | Injects phase, score, and recent decisions into each prompt |
| **phase-gate** | `PreToolUse` (Edit/Write) | Blocks code editing in Phases 0-4 |
| **loop-detector** | `PreToolUse` (Bash) | Detects repetitive test executions (>3x) |

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

## LLM Compatibility

The IACDM methodology demands strict instruction following, reliable MCP tool calling, and sustained reasoning across long contexts. Not all models meet these requirements equally.

### Selection Criteria

| Criterion | Why it matters |
|---|---|
| Instruction following | Each phase has detailed behavioral rules (~2-4k tokens). Models that summarize or skip steps break the methodology |
| MCP tool reliability | The methodology relies on 15 tools being called consistently. Models that "narrate" tool calls instead of executing them are incompatible |
| Context window | Phases 0-2 accumulate significant context. Minimum 32k tokens, recommended 128k+ |
| Reasoning depth | Phases 0-4 (design) require architectural analysis, not code generation. Shallow reasoning produces shallow designs |

### Model Tier Classification

| Tier | Model | Phases 0-4 (Design) | Phases 5-7 (Code) | Notes |
|---|---|---|---|---|
| S | Claude Opus | Excellent | Excellent | Best instruction following. Highest cost |
| A | Claude Sonnet | Very good | Very good | Best cost/quality balance. Occasionally simplifies Phase 2 critique |
| C | Claude Haiku | Not recommended | Acceptable | Skips safeguards, simplifies critique, ignores R5. Only for Phases 5-7 with validated architecture |

### Recommended Strategy (LLM Switch Point)

The methodology includes a natural **LLM Switch Point at Phase 4**. The architecture is fully validated and persisted in `state.json` + `specs/`, so no context is lost when switching models.

| Strategy | Phases 0-4 | Phases 5-7 | Best for |
|---|---|---|---|
| Maximum quality | Opus | Opus | Critical/high-complexity projects |
| Optimal cost/quality | Opus | Sonnet | Most projects — leverages the LLM Switch Point |
| Budget | Sonnet | Sonnet | Medium-complexity projects |

> **Rule of thumb:** The cost of architectural error (detected in Phase 5-6) is 10-100x the cost of using a more capable model in Phases 0-4. Invest in reasoning where reasoning matters.

### Built-in Protections for Weaker Models

| Mechanism | Protects against |
|---|---|
| Gateway Guard | Model forgetting to load context (>30min without `get_phase_state`) |
| Phase Gate hook | Code editing in Phases 0-4 (blocked regardless of model) |
| Loop Detector | Repetitive patterns (>3x same action) — safeguard S7 |
| Compact Guidance | Summarized instructions injected into every prompt via hook |
| Phase 7 Engine Hint | Forces meta-iteration offer even if model didn't read guidance |

## Requirements

| Requirement | Minimum | Why |
|---|---|---|
| **VS Code** | >= 1.85.0 | Extension API compatibility |
| **Node.js** | >= 18.0.0 | Runs the MCP server (`node .versus/server.js`) |
| **Claude Code** | Latest | Extension + CLI for MCP and hooks |

> **Node.js is mandatory.** Without it, the MCP server cannot start and Claude will ignore the methodology entirely. Install from [nodejs.org](https://nodejs.org).

## Companion Extension

Use **[Versus for Copilot](https://marketplace.visualstudio.com/items?itemName=jasminemoreira.versus-copilot)** for GitHub Copilot Agent Mode integration. Both extensions share the same `.versus/state.json` format.

## Changelog

### v0.3.2 — 2026-02-27

**Added:**
- **Spec-Driven Test Protocol (Phase 6):** Tests must map to specs, not implementation. Test Map (MUST DISPLAY), mandatory negative tests (1:2 ratio), distinction between "test green" vs "spec met", spec coverage report as traceable decision
- **LLM Compatibility section** in README with Claude-specific model tiers (Opus S / Sonnet A / Haiku C), LLM Switch Point strategy, and built-in protections table
- **Phase 3 post-cycle decision rewrite:** AI computes structural change % and criticals, presents recommendation, user confirms (instead of asking user to choose percentage range)
- **Phase 5 autonomous implementation:** "Do NOT ask permission between files" — implement sequentially, stop only for blockers

**Changed:** Publisher ID to `JasmineMoreira`. Repository/homepage URLs point to public repo.

**Removed:** R3 (One artifact per response) — removed from guidance and SKILL.md.

### v0.3.1 — 2026-02-27

**Added:**
- **`start_new_cycle` MCP tool (#15):** Meta-iteration v1→v2 without losing context. Preserves decisions, projectSpec, history, specs/
- **Phase 7 engine-level hints:** `_phase7Action` in `get_phase_state` and `mark_exit_criterion` responses forces meta-iteration offer regardless of guidance reading
- **Delivery Target question (Phase 0 Level 5):** Mandatory question — Complete product / MVP / Prototype. Conditions Phase 1 Progressive Scope
- **R5 Mandatory Display:** Global rule forcing display of essential outputs with MUST DISPLAY markers across all phases
- **Auto-approved permissions:** Extension configures Read, Write, Edit, Glob, Grep, Bash, WebFetch, WebSearch, NotebookEdit alongside MCP tools
- **Node.js requirement:** Added to README Quick Start and package.json engines

**Changed:** Phase 1 Progressive Scope conditioned on delivery target. Phase 5 S7 reinforced for autonomous implementation.

**Fixed:** Phase 7 end-of-cycle — `isValidTransition(7, 0)` returned false and `initProject()` wiped context. New dedicated `startNewCycle()` method solves both.

### v0.3.0 — 2026-02-26

Initial stable release: 14 MCP tools, 3 Claude Code hooks (inject-context, phase-gate, loop-detector), Gateway Guard, VS Code sidebar, multi-session testing protocol.

## License

[MIT](LICENSE) - Created by [Jasmine Moreira](https://github.com/jasminemoreira).
