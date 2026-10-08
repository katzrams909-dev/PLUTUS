# PLUTUS

PLUTUS is an institutional volume, value, participation, and reaction-intelligence indicator for TradingView.

## Release Status

**PLUTUS v1.0 release candidate is frozen.**

Release baseline:
`c2e8d0860164f98b372038f177a92c0bf9d0bfae`

Primary release file:
`pine/release/PLUTUS_RC1.pine`

The trading model is frozen at this baseline. Further changes should be limited to demonstrated compile/runtime, repaint, lifecycle, direction, provenance, or release-blocking defects.

## Project Thesis

PLUTUS is designed to answer a central question:

> Where is meaningful capital participating, and is that participation supporting or opposing the current price movement?

The system combines volume intelligence, VWAP, volume profile/value analysis, institutional price structures, divergence, regime classification, and interaction-first signals in a modular Pine Script v6 framework.

## Engine Sequence

1. P01 — Volume Intelligence
2. P02 — VWAP
3. P03 — Volume Profile
4. P04 — Value Analysis
5. P05 — Institutional Zones
6. P06 — Divergence
7. P07 — Confluence
8. P08 — Visual / UX
9. P09 — Signal Engine
10. P10 — Integration / Release Hardening

## v1.0 Signal Model

PLUTUS evaluates:

- FVG / IFVG
- Order Blocks / Breaker Blocks
- institutional clusters
- 1–3 bar zone reactions
- wick-only zone interactions
- post-flip retests for IFVG / Breaker
- volume and effort-vs-result
- ADX / DI regime
- divergence
- Trend / Countertrend / Reversal classification
- liquidity and reward space
- setup-centric Signal Quality (SQ)

Reaction classes include:

- FVG_REJECTION
- FVG_SWEEP_RECLAIM
- IFVG_RETEST
- OB_REJECTION
- OB_SWEEP_RECLAIM
- BREAKER_RETEST
- ZONE_FAILURE
- CLUSTER_REJECTION
- CLUSTER_SWEEP_RECLAIM

See:
- `docs/PROJECT_SPEC.md`
- `docs/ENGINE_ARCHITECTURE.md`
- `docs/RELEASE_v1.0.md`
- `docs/USER_GUIDE.md`
- `tests/regression/VALIDATION_PROTOCOL.md`
- `tests/regression/RC1_RELEASE_AUDIT.md`
