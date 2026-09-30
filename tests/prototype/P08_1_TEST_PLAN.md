# PLUTUS P08.1 — Visual Refinement / Production UX Test Plan

## Objective
Validate the production visual refinement layer without changing P07 scoring semantics.

## Compile / Runtime
- Pine Script v6 compiles without errors.
- No runtime errors on long histories.
- No unbounded label, line, or box accumulation.

## Presets
Validate all display presets:
- Minimal
- Balanced
- Full
- Diagnostic

Expected:
- Minimal removes non-essential chart context.
- Balanced remains clean for normal use.
- Full exposes richer context without excessive overlap.
- Diagnostic exposes divergence labels and detailed diagnostics.

## Visual Hierarchy
- Bullish context uses consistent green/lime hierarchy.
- Bearish context uses consistent red hierarchy.
- VWAP remains visually distinct.
- Profile/value visuals remain subordinate to primary bias and zones.
- Conflict/low-confidence states are visually distinguishable from directional bias.

## VWAP
- Daily VWAP explicitly breaks at day reset.
- No vertical reset joins.
- Hiding VWAP does not affect confluence calculations.

## Profile / Histogram
- Profile centre is readable in Balanced/Full/Diagnostic.
- Value envelope appears only in Full/Diagnostic.
- Histogram is toggleable and hidden completely when disabled.
- Histogram uses a fixed reusable box pool.

## Institutional Zones
- Only the current bullish and bearish visual inventory boxes are retained.
- Older zone boxes are replaced by newer ones.
- Boxes expire after the configured zone lookback.
- Visual opacity/border treatment fades with age.
- Stale zones remain distinguishable from fresh zones.
- Hiding zones does not change P05 scoring state.

## Divergence
- Divergence drawings remain bounded by Maximum Divergence Events.
- Full/Diagnostic presets show divergence events as configured.
- Diagnostic mode adds compact divergence labels.
- Hidden divergence visuals do not change divergence scoring.

## Transition Markers
- Material transitions are bounded by Maximum Transition Labels.
- Cooldown suppresses clustered duplicate markers.
- Labels are offset away from candle highs/lows to reduce overlap.
- Directional, conflict, and low-confidence transitions remain visually distinct.

## Panels
- Production status panel stays compact and readable.
- Diagnostic panel remains separate from production panel.
- Diagnostic panel reports current object counts.

## Theme QA
Check both dark and light chart backgrounds for:
- table readability
- VWAP contrast
- profile contrast
- zone visibility
- divergence visibility
- transition label readability

## Long-History Object QA
Run on at least:
- NAS100 / US100
- XAUUSD
- EURUSD
- BTCUSD

Use multiple intraday timeframes and verify object counts remain bounded and chart interaction remains responsive.

## Acceptance Gate
P08.1 passes when it compiles cleanly, all four display presets behave as intended, reset-safe plotting remains intact, and no visual control changes the underlying confluence logic.
