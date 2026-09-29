# PLUTUS P05.3 — Unified Institutional Zone Engine Test Plan

## Objective
Validate the consolidated P05 institutional-zone engine as the authoritative downstream source for FVG, IFVG, OB and Breaker context.

## Compile gate
- Pine Script v6 compiles without errors.
- No runtime array errors.
- No object-limit errors under normal intraday use.

## Candidate integrity
- FVG requires a genuine three-candle wick imbalance plus middle-candle displacement.
- Opening-gap artifacts are rejected when the anti-gap filter is enabled.
- Nested/overlapping active FVG-family zones are suppressed.
- OB requires an opposing candle followed by displacement through recent structure.
- Candidate width filters remain bounded by ATR and tick settings.

## Qualification
- Birth score remains bounded 0–100.
- P01 participation, P02 fair value, P03 profile and P04 value context contribute directionally.
- `Show Only Qualified Zones` affects drawings only, not internal registry state.

## Lifecycle
- Fresh -> partial -> mitigated / invalidated / expired transitions behave as in accepted P05.2.
- Current score decays with age, mitigation and retests.
- Birth score remains unchanged.
- Wick/Close mitigation basis changes penetration handling as expected.
- Invalidated and expired boxes stop extending immediately.

## IFVG / Breaker
- Only original FVG may arm IFVG conversion.
- Only original OB may arm Breaker conversion.
- Inversion requires decisive break and later confirmation.
- No immediate same-bar inversion.
- Inverted zones are re-qualified in the new direction.
- IFVG / Breaker zones do not recursively invert.

## Unified active-zone selection
For all active zones:
- strongest bullish zone is selected by current score.
- strongest bearish zone is selected by current score.
- reported zone type and price bounds match the selected box.
- active bullish/bearish counts are correct.

## Institutional Zone Score
- Score is bounded -100 to +100.
- positive = bullish zone advantage.
- negative = bearish zone advantage.
- equal opposing strengths produce approximately zero.
- missing opposite-side zones do not create `na` output.

## Unified states
Validate:
- `ZONE_UNAVAILABLE`
- `ZONE_CONFLICT`
- `ZONE_STRONGLY_BULLISH`
- `ZONE_STRONGLY_BEARISH`
- `ZONE_BULLISH`
- `ZONE_BEARISH`
- `ZONE_LEANING_BULLISH`
- `ZONE_LEANING_BEARISH`
- `ZONE_BALANCED`
- live bar `FORMING`

## Conflict handling
- Conflict requires both sides to exceed `Strong Active Zone Score`.
- Their score difference must be within `Opposing Zone Conflict Tolerance`.
- Strong but clearly dominant one-sided context must not be classified as conflict.

## Profile / VWAP rendering
- Primary VWAP breaks at selected session/day/week/month reset.
- POC/VAH/VAL break at selected profile reset.
- No vertical joins across resets.

## Histogram
- on/off toggle removes all histogram boxes when disabled.
- `Range Overlap` allocation renders a continuous distribution.
- `HLC3 Point` remains available for comparison/debugging.
- row count changes visual resolution without changing analytical POC/VAH/VAL bins.
- no stale boxes after toggling.

## Diagnostics
Confirm table correctly reports:
- unified state
- Institutional Zone Score
- bullish/bearish active counts
- best bullish zone type/score/range
- best bearish zone type/score/range
- conflict flag
- P01/P02/P03/P04 context
- armed inversions
- histogram configuration
- profile scope
- tracked-zone count

## Cross-asset sanity
Run at minimum on:
- NAS100 / index CFD
- XAUUSD
- one FX pair
- one crypto pair

Test multiple intraday timeframes and verify stable lifecycle behavior, bounded objects and no obvious false-zone explosion.
