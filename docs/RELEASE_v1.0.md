# PLUTUS v1.0 — Release Freeze

## Frozen RC Baseline
`c2e8d0860164f98b372038f177a92c0bf9d0bfae`

Primary indicator:
`pine/release/PLUTUS_RC1.pine`

## Release Position
PLUTUS v1.0 is feature-frozen. The current trading model is accepted for release-candidate use.

Post-freeze changes should be limited to:
- Pine compile/runtime defects
- confirmed repaint defects
- wrong-direction signals
- lifecycle/state corruption
- source/provenance errors
- object/runtime release blockers

Threshold tuning or feature expansion should be deferred to a later version unless required to correct a demonstrated defect.

## Core Capabilities
- Pine Script v6
- no native `request.footprint()` dependency
- relative participation / RVOL
- effort-versus-result / absorption
- VWAP references
- volume profile / value analysis
- FVG / IFVG
- Order Block / Breaker
- institutional clusters
- divergence
- ADX / DI regime qualification
- Trend / Countertrend / Reversal signals
- 1–3 bar reaction confirmation
- wick-only interaction
- IFVG / Breaker post-flip retest requirement
- multi-zone pending reaction observation
- liquidity / reward-space gating
- setup-centric SQ
- signal audit diagnostics

## Accepted Behavioral Rules
1. Zone type and trade regime are independent.
2. IFVG / Breaker cannot signal on their flip candle.
3. IFVG / Breaker require a later retest.
4. Wick overlap is a valid zone interaction.
5. A first retest candle may close inside the zone; bar 2 or 3 may provide confirmation.
6. Earliest valid confirmation wins.
7. Multiple zones may remain pending concurrently.
8. Completed reactions, not incomplete first touches, are arbitrated for final signal selection.
9. Hard vetoes override SQ.
10. Hidden visuals do not intentionally alter signal logic.

## Known Design Limits
- Main PLUTUS does not use `request.footprint()`.
- Profile calculations are PLUTUS approximations and do not claim exact TradingView proprietary profile equivalence.
- FX/CFD volume is interpreted as relative participation and may be broker/feed dependent.
- Zone capacity remains bounded and currently retains the prior simple capacity behavior after the quality-aware eviction experiment was reverted.
- Further broad historical testing remains useful even after feature freeze.

## Release Verification
See:
`tests/regression/RC1_RELEASE_AUDIT.md`
