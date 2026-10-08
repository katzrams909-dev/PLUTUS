# PLUTUS Engine Architecture

## Engine Stack

### P01 — Volume Intelligence
Relative volume, participation, effort-versus-result, expansion/contraction, absorption, and low-participation states.

### P02 — VWAP
Session/daily/weekly/monthly fair-value references and value-state interpretation.

### P03 — Volume Profile
POC / VAH / VAL and profile distribution using PLUTUS allocation methods.

### P04 — Value Analysis
Value migration, acceptance/rejection, overlap, separation, and auction state.

### P05 — Institutional Zones
FVG, IFVG, Order Block, and Breaker inventory with lifecycle, mitigation, inversion, and stable provenance.

### P06 — Divergence
Regular/hidden divergence used as a capped global modifier and, where relevant, local institutional-reaction evidence.

### P07 / P10 — Confluence
Three primary pillars:
1. Participation
2. Value / Auction
3. Institutional Inventory

Divergence remains a capped secondary modifier rather than an equal fourth pillar.

### P08 — Visual / UX
VWAP, profile, zones, sessions, status panels, diagnostics, and signal presentation.

### P09 / P10 — Signal Engine
Interaction-first signal workflow with explicit reaction and regime classification.

## Final Signal Architecture

`LIVE ZONE → TOUCH → PENDING REACTION → COMPLETED REACTION → ARBITRATION → CONTEXT / REGIME / HARD GATES → SQ → SIGNAL`

### Pending Reaction Inventory
Multiple zones may be observed concurrently.

A pending reaction stores:
- stable source ID / cluster pair
- direction
- reaction class
- bounds
- source quality
- reaction start/deadline
- wick evidence
- best absorption
- best RVOL
- best aligned divergence
- provenance

The confirmation window is 1–3 bars. The earliest valid confirmation wins for a given setup.

### Post-Flip Rule
IFVG and Breaker creation bars are never retest bars. A flipped zone becomes signal-eligible only on a later bar.

### Wick Rule
Signal interaction uses wick overlap. A wick may enter a zone and close back outside without the candle body entering the zone.

### Regime Classification
Reaction and trade regime are independent.

Reaction:
- FVG_REJECTION / FVG_SWEEP_RECLAIM
- IFVG_RETEST
- OB_REJECTION / OB_SWEEP_RECLAIM
- BREAKER_RETEST
- ZONE_FAILURE
- cluster reaction variants

Regime:
- TREND
- COUNTERTREND
- REVERSAL

A bearish OB formed near the top of a bull trend may first produce a countertrend or reversal short, and later act as trend-aligned bearish inventory after regime transition.

## Final Quality / Veto Model
Final SQ is setup-centric and combines:
- source quality
- reaction quality
- participation/flow
- regime compatibility
- context/confidence
- available reward space

Hard vetoes are separate and cannot be mathematically overridden by SQ.

## Release Freeze
Frozen RC baseline:
`c2e8d0860164f98b372038f177a92c0bf9d0bfae`

Post-freeze modifications are reserved for demonstrated defects, not discretionary threshold tuning.
