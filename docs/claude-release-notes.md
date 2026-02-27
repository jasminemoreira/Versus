# Versus for Claude — Release Notes

Detailed release notes for the Claude Code integration of the IACDM methodology.

**Install:** [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=JasmineMoreira.versus-claude)

---

## v0.3.2 — 2026-02-27

### Spec-Driven Test Protocol (Phase 6)

The biggest change in this release addresses a fundamental problem: **the AI tends to write tests that mirror the implementation instead of verifying the specs.**

Phase 6 now follows a mandatory 4-step protocol:

1. **Recover scope** — Before writing any test, the AI must retrieve Phase 0 use cases (`get_decisions(phase=0)`), read `specs/validation`, and read `specs/technical`
2. **Build Test Map** (MUST DISPLAY) — A table mapping every spec item to its test, declaring HOW it verifies the spec and whether it's positive or negative
3. **Record spec coverage** — A traceable decision (`record_decision(category='spec-coverage')`) documenting what's covered, what's not, and where there are gaps
4. **Manual exploratory testing** — Unchanged, still mandatory

Key rules:
- Every use case: at least 1 positive + 1 negative test
- Minimum ratio: 1 negative test for every 2 positive tests
- Explicit distinction between "test green" and "spec met" — a test that passes without verifying the exact criterion is false coverage

### Phase 3 Post-Cycle Decision

Previously, the methodology asked the user to choose a structural change percentage range (<15% / 15-25% / >25%). Problem: the user has no way to know this — only the AI can compute it.

Now follows a 3-step protocol:
1. **AI computes** — compares V(N) with V(N+1), calculates structural change %, counts remaining criticals
2. **AI presents** — shows computed values + reference table + recommended action
3. **User confirms** — validates the recommendation (accept / override), not the raw data

### Phase 5 Autonomous Implementation

Added explicit instruction: "Do NOT ask permission between files — implement sequentially and autonomously." The AI should only stop for actual blockers or decisions, not to ask "should I continue?"

### R3 Removed

The "one artifact per response" rule (R3) was removed. It was a legacy constraint that no longer applies to Claude Code's interaction model.

### Other Changes
- Publisher ID: `JasmineMoreira`
- Repository/homepage URLs now point to the public repo
- LLM Compatibility section added to README

---

## v0.3.1 — 2026-02-27

### Meta-iteration: `start_new_cycle` MCP Tool

After completing Phase 7, the methodology should offer a new development cycle (v1→v2). Previously this was broken:
- `isValidTransition(7, 0)` returned false
- `initProject()` wiped all context

New `startNewCycle()` engine method and `start_new_cycle` MCP tool (tool #15) solve this. The tool:
- Only works from Phase 7 with all exit criteria met
- Preserves: decisions, projectSpec, history, specs/
- Resets: phase→0, iteration→1, score→null, exitCriteria→[], safeguards→fresh

Engine-level hints (`_phase7Action`) were added to `get_phase_state` and `mark_exit_criterion` responses to force the meta-iteration offer even if the model didn't read the guidance.

### Delivery Target (Phase 0)

Added mandatory question in Phase 0 Level 5: "What is the delivery target for this project?" with options:
- Complete product (blocks scope splitting unless technical blocker)
- MVP first, then iterate
- Prototype / proof of concept

This prevents the AI from silently reducing scope to MVP when the user wants a complete product.

### R5 Mandatory Display

New global rule forcing display of essential outputs with MUST DISPLAY markers:
- P0: Score breakdown + teach-back + delivery target
- P2: Coverage matrix + concentration analysis + exit gate
- P3: Post-cycle decision + anti-scope-creep checklist
- P4: Convergence report + LLM switch notification
- P6: Test Map + spec coverage + testing status
- P7: Double-loop report + consolidation table + meta-iteration offer

### Auto-Approved Permissions

The extension now configures all development permissions automatically: Read, Write, Edit, Glob, Grep, Bash, WebFetch, WebSearch, NotebookEdit — alongside the 15 MCP tool permissions.

### Node.js Requirement

Added Node.js >= 18.0.0 to requirements. The MCP server runs via `node .versus/server.js` — without Node.js installed, Claude ignores the methodology entirely.

---

## v0.3.0 — 2026-02-26

Initial stable release.

- 14 MCP tools for state management, decision recording, and phase transitions
- 3 Claude Code hooks: inject-context (UserPromptSubmit), phase-gate (PreToolUse), loop-detector (PreToolUse)
- Gateway Guard: detects when models skip loading context (>30min without `get_phase_state`)
- VS Code sidebar showing methodology state in real time
- Multi-session testing protocol for Phase 6 continuity
- Full IACDM methodology v3.6 embedded in guidance
