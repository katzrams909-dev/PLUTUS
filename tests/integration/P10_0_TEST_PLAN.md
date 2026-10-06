# PLUTUS P10.0 — Integrated Core Test Plan

## Objective
Validate the first production integration core before P09.3 signal-state grafting.

P10.0 preserves the accepted P08.2 visual shell and replaces the old direct P01-P06 top-level confluence average with the anti-double-counting three-pillar architecture.

## Architecture
Primary pillars:
1. Participation — P01
2. Value / Auction — P02 + P03 + P04 blended internally
3. Institutional Inventory — P05

Secondary modifier:
- P06 Divergence — capped additive modifier, never an equal fourth pillar

## Compile / Runtime
- [ ] Pine Script v6 compiles without errors.
- [ ] No runtime array/object errors.
- [ ] No request.footprint() usage.
- [ ] Session/profile/VWAP visuals remain bounded and reset-safe.
- [ ] POC/VAH/VAL remain internal calculations and are not standalone chart plots.

## Three-Pillar Confluence
- [ ] P01 contributes only through Participation pillar.
- [ ] P02/P03/P04 contribute only through the Value/Auction pillar at the top level.
- [ ] P05 contributes only through Institutional Inventory.
- [ ] P06 can modify the base score only up to the configured max points.
- [ ] P06 alone cannot create a strong directional score when all three pillars are neutral.
- [ ] Unavailable pillar evidence receives zero effective top-level weight.
- [ ] Value pillar dynamically reweights around available P02/P03/P04 evidence.
- [ ] Top-level score remains clamped to -100..100.

## Confidence
- [ ] Confidence reflects pillar availability.
- [ ] Confidence reflects pillar reliability.
- [ ] Confidence reflects directional agreement.
- [ ] Cross-pillar disagreement applies the configured conflict penalty.
- [ ] Low active/reliable pillar count produces LOW_CONFIDENCE state.

## Hysteresis / State
- [ ] Confirmed-bar state transitions are preserved.
- [ ] Bullish state persists until exit threshold is broken.
- [ ] Bearish state persists until exit threshold is broken.
- [ ] Material transition labels remain finite and cooldown-limited.

## Visual Regression
- [ ] P08.2 session boxes remain visually unchanged.
- [ ] Asia / London / NY AM+PM / Full NY display modes still function.
- [ ] VWAP reset breaks remain unchanged.
- [ ] Unified volume profile histogram remains driven by the shared profile grid.
- [ ] POC/VAH/VAL numeric values remain available to status/diagnostics.
- [ ] Institutional-zone visuals remain finite.
- [ ] Divergence visuals remain finite.
- [ ] Display presets do not alter calculations.

## Status / Diagnostics
- [ ] Status panel shows score, confidence, profile, zones and active session.
- [ ] Active/reliable counts now refer to pillars, not six independent engines.
- [ ] Diagnostics still expose raw P01-P06 scores for engineering review.
- [ ] Diagnostic engine weights are understood as internal/sub-pillar evidence weights, not top-level voting weights.

## Known P10.1 Work
P10.0 intentionally does not yet graft the validated P09.3 signal state machine. P10.1 must:
- port the latest strict zone lifecycle from P09.3,
- preserve zone IDs/provenance,
- integrate REJECTION / SWEEP_RECLAIM / ZONE_FAILURE / FLIP_RETEST signal classes,
- use P10 pillar score/confidence instead of P09 compact confluence proxy,
- preserve direction/type invariants,
- preserve finite signal/risk/reward visuals and alerts.

## Acceptance Gate
P10.0 passes when:
1. It compiles cleanly.
2. P08 visual behavior is preserved.
3. Confluence behavior is materially different from the old six-engine direct average in overlapping P02/P03/P04 cases.
4. Divergence behaves as a capped modifier.
5. No hidden display toggle changes logic.
