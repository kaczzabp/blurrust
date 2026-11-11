# Architecture decisions

## ADR-001: Layered workspace

**Decision:** `blur-formats` has no engine dependency; `blur-assets` converts user-owned source data into a versioned cache; simulation, rendering, UI, audio, networking, and app orchestration are separate crates.

**Why:** parsers remain testable with synthetic fixtures and asset semantics do not leak into presentation code.

## ADR-002: One deterministic simulation funnel

**Decision:** gameplay state changes only through fixed-step `blur_sim::step(state, input) -> state`. Server authority, prediction, replay, trace fitting, and tests will call the same function.

**Status:** architecture is fixed; original timestep value is **UNKNOWN** and not yet encoded.

## ADR-003: Evidence boundary

**Decision:** original files are read-only runtime input. Raw reverse-engineering material and captures are ignored; only cleaned, paraphrased, address-free conclusions and synthetic fixtures are tracked.

## ADR-004: Phase-gated dependencies

**Decision:** Phase 0 creates compiling crate boundaries without adding Bevy/wgpu. Those dependencies enter only when Phase 1 evidence and the relevant phase gate justify them.

