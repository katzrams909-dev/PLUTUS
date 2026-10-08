# PLUTUS Project Specification

## Mission
PLUTUS is a TradingView indicator focused on institutional participation, value distribution, volume-informed market behaviour, and reaction-based execution context.

## Core Question
Where is meaningful capital participating, and is that participation supporting or opposing the current price movement?

## Primary Domains
- Volume intelligence and relative participation
- VWAP and institutional fair value
- Volume profile and auction value
- Order Blocks / Breaker Blocks
- FVG / IFVG
- Volume- and momentum-based divergence
- Institutional confluence and regime classification
- Interaction-first signal generation
- Trend / Countertrend / Reversal classification

## Design Principles
- Non-repainting confirmed-close workflow where technically applicable
- Modular engines developed and validated independently before integration
- Minimal chart clutter
- Diagnostics separated from production visuals
- Signals are downstream outputs, not the starting point
- SMC constructs are strengthened by participation/value evidence
- Zone type does not determine trade regime
- A touch starts investigation; confirmation decides whether a trade exists
- Hard invalidations cannot be rescued by SQ
- IFVG / Breaker require a later retest after the flip candle
- Wick-only zone interaction is valid interaction
- Multi-bar reactions may confirm on bar 1, 2, or 3

## Architecture
P01 Volume Intelligence  
P02 VWAP  
P03 Volume Profile  
P04 Value Analysis  
P05 Institutional Zones  
P06 Divergence  
P07 Confluence  
P08 Visual / UX  
P09 Signals  
P10 Integration and Release Hardening

## v1.0 Frozen Baseline
Release candidate:
`c2e8d0860164f98b372038f177a92c0bf9d0bfae`

Primary file:
`pine/release/PLUTUS_RC1.pine`

The trading model is frozen at this baseline. Post-freeze changes require a demonstrated defect or release blocker.

## Signal Taxonomy

### Reaction
- FVG_REJECTION
- FVG_SWEEP_RECLAIM
- IFVG_RETEST
- OB_REJECTION
- OB_SWEEP_RECLAIM
- BREAKER_RETEST
- ZONE_FAILURE
- CLUSTER_REJECTION
- CLUSTER_SWEEP_RECLAIM

### Regime
- TREND
- COUNTERTREND
- REVERSAL

Reaction type describes what price did at institutional inventory. Regime describes what kind of trade the reaction represents.

## Final Decision Order

`source validity → interaction → 1–3 bar reaction → participation/ER → ADX/DI → divergence → regime → liquidity/reward space → hard vetoes → SQ → signal`

For overlapping zones, multiple pending reactions may be tracked concurrently and completed reactions are arbitrated before entering the downstream confirmation stack.
