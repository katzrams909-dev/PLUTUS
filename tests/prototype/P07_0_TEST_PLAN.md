# PLUTUS P07.0 — Confluence Baseline Test Plan

## Objective
Validate the first integration layer combining compact P01–P06 directional evidence into a unified confluence score and confidence state without generating trade signals.

## Compile gate
- Pine Script v6 compiles without errors.
- Indicator loads without runtime errors.
- Diagnostic table renders when enabled.

## Functional checks
1. Confirm all six engine rows populate: P01 Participation, P02 Fair Value, P03 Profile, P04 Value Analysis, P05 Institutional Zones, P06 Divergence.
2. Confirm each engine score remains bounded to -100..100.
3. Confirm configurable weights alter the unified score direction/magnitude without errors.
4. Confirm `Engine Count` reflects only engines whose absolute score exceeds `Engine Active Threshold`.
5. Confirm bullish/bearish counts reflect active directional engines.
6. Confirm bullish and bearish evidence totals remain non-negative.
7. Confirm `Confluence` remains bounded to -100..100.
8. Confirm `Confidence` remains bounded to 0..100.
9. Confirm confidence falls when `CONFLUENCE_CONFLICT` is active.
10. Confirm no entry/exit signals, alerts, or strategy orders are generated.

## State checks
Expected confirmed states:
- `CONFLUENCE_STRONGLY_BULLISH`
- `CONFLUENCE_BULLISH`
- `CONFLUENCE_NEUTRAL`
- `CONFLUENCE_BEARISH`
- `CONFLUENCE_STRONGLY_BEARISH`
- `CONFLUENCE_CONFLICT`
- `FORMING` on an unconfirmed realtime bar

## Conflict checks
- Create/observe periods where multiple engines disagree directionally.
- Verify conflict requires both bullish and bearish evidence to exceed the configured side minimum and remain within the configured score-difference tolerance.
- Verify conflict penalty affects confidence, not the raw signed engine calculations.

## Visual checks
- Diagnostic table should remain compact and legible.
- Optional current-bias label can be toggled on/off cleanly.
- No persistent chart objects should accumulate when bias label is enabled.

## Regression notes
P07.0 uses compact in-file representations of validated P01–P06 logic to test aggregation architecture. It is not yet the final merged production indicator. Exact engine implementations will be ported/reconciled during integration/optimization after the confluence architecture is validated.
