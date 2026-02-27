# Changelog

All notable changes to Versus extensions are documented here.

Format: `[Platform Version]` — changes grouped by type.

---

## [Claude 0.3.2] — 2026-02-27

### Added
- **Spec-Driven Test Protocol (Phase 6):** Tests must map to specs, not implementation. Test Map (MUST DISPLAY), mandatory negative tests (1:2 ratio), distinction between "test green" vs "spec met", spec coverage report as traceable decision
- **LLM Compatibility section** in README with Claude-specific model tiers (Opus S / Sonnet A / Haiku C), LLM Switch Point strategy, and built-in protections table
- **Phase 3 post-cycle decision rewrite:** AI computes structural change % and criticals, presents recommendation, user confirms (instead of asking user to choose percentage range)
- **Phase 5 autonomous implementation:** "Do NOT ask permission between files" — implement sequentially, stop only for blockers

### Changed
- Publisher ID updated to `JasmineMoreira`
- Repository/homepage URLs point to public repo (github.com/jasminemoreira/Versus)

### Removed
- **R3 (One artifact per response)** — removed from guidance and SKILL.md

---

## [Claude 0.3.1] — 2026-02-27

### Added
- **`start_new_cycle` MCP tool (#15):** Meta-iteration v1→v2 without losing context. Preserves decisions, projectSpec, history, specs/
- **Phase 7 engine-level hints:** `_phase7Action` in `get_phase_state` and `mark_exit_criterion` responses forces meta-iteration offer regardless of guidance reading
- **Delivery Target question (Phase 0 Level 5):** Mandatory question — Complete product / MVP / Prototype. Conditions Phase 1 Progressive Scope
- **R5 Mandatory Display:** Global rule forcing display of essential outputs with MUST DISPLAY markers across all phases
- **Auto-approved permissions:** Extension configures Read, Write, Edit, Glob, Grep, Bash, WebFetch, WebSearch, NotebookEdit alongside MCP tools
- **Node.js requirement:** Added to README Quick Start and package.json engines

### Changed
- Phase 1 Progressive Scope conditioned on delivery target — "Complete product" blocks scope splitting unless technical blocker
- Phase 5 S7 reinforced for autonomous implementation

### Fixed
- Phase 7 end-of-cycle: `isValidTransition(7, 0)` returned false and `initProject()` wiped context. New dedicated `startNewCycle()` method solves both

---

## [Claude 0.3.0] — 2026-02-26

### Added
- Initial stable release
- 14 MCP tools for state management, decision recording, phase transitions
- 3 Claude Code hooks (inject-context, phase-gate, loop-detector)
- Gateway Guard for context loading detection
- VS Code sidebar with methodology state
- Multi-session testing protocol for Phase 6

---

## [Copilot 0.3.2] — 2026-02-27

### Added
- **Spec-Driven Test Protocol (Phase 6):** Same as Claude version — tests against specs, Test Map, negative tests, spec coverage report
- **LLM Compatibility section** in README with multi-model tiers (S/A/B/C), LLM Switch Point strategy, built-in protections
- **Phase 3 post-cycle decision rewrite:** AI computes, presents, user confirms

### Changed
- R2 template adapted for chat-based interaction (vs AskUserQuestion)

### Removed
- **R3 (One artifact per response)**
