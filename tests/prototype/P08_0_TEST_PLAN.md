# PLUTUS P08.0 — Visual Baseline Test Plan

## Objective
Validate that the P08.0 presentation layer improves chart usability without changing the underlying P07.2 confluence intent or introducing uncontrolled chart objects.

## 1. Compile / Runtime
- Pine Script v6 compiles without errors.
- No runtime errors on NAS100/US100, XAUUSD, EURUSD and BTCUSD.
- Test on multiple intraday timeframes and at least one higher timeframe.

## 2. Display Presets
Verify all presets recalculate cleanly:
- Minimal
- Balanced
- Full
- Diagnostic

Expected hierarchy:
- Minimal: compact status-focused presentation.
- Balanced: status + VWAP/profile + zones.
- Full: adds divergence/histogram-capable detail.
- Diagnostic: full visibility plus detailed diagnostics.

## 3. Status Panel
- Panel can be toggled independently.
- Bias, score, confidence, regime, engine counts, support and opposing engine are readable.
- Conflict / neutral / bullish / bearish presentation is visually distinguishable.
- Panel does not create chart labels every bar.

## 4. VWAP
- VWAP toggle works.
- Daily VWAP does not draw a connecting line across the daily reset.
- Hiding VWAP does not alter confluence calculations.

## 5. Profile Visuals
- Profile Centre toggle works.
- Upper/lower value envelope appears only in Full/Diagnostic when profile visibility is enabled.
- Hiding profile visuals does not alter P03/P04 scoring.

## 6. Histogram
- Histogram is OFF by default.
- Histogram can be toggled on in Full/Diagnostic presets.
- Histogram fully disappears when toggled off.
- Box pool is reused rather than continuously creating boxes.
- Histogram remains bounded by configured row count.
- Changing Histogram Rows causes a clean script recalculation.

## 7. Institutional Zones
- Latest bullish and bearish zone visuals are clearly distinguishable.
- A newly detected same-direction zone replaces the older displayed same-direction zone.
- Expired zone boxes are removed after the configured zone lookback.
- Hiding zones removes zone objects without affecting P05 context logic.

## 8. Divergence Visuals
- Divergence visuals are only shown in Full/Diagnostic unless explicitly disabled.
- Bullish and bearish divergence lines are visually distinct.
- Diagnostic divergence labels include score.
- Stored divergence lines/labels remain bounded by Maximum Divergence Events.

## 9. Transition Labels
- Material transition labels only appear on confirmed material transitions.
- Transition labels are bounded by Maximum Transition Labels.
- Minimal preset suppresses transition labels.
- Hiding transitions does not affect state logic.

## 10. Bias Background
- OFF by default.
- Only active in Full/Diagnostic when enabled.
- Background treatment remains subtle enough to preserve candle readability.

## 11. Diagnostic Separation
- Detailed diagnostics are separate from the compact production status panel.
- Diagnostic preset automatically enables detailed diagnostics.
- Turning diagnostics off in non-Diagnostic presets clears the diagnostic table.

## 12. Object Discipline
- No unbounded boxes, lines or labels.
- Old transition labels are deleted when the configured maximum is exceeded.
- Old divergence objects are deleted when the configured maximum is exceeded.
- Zone boxes are replaced/removed rather than accumulating indefinitely.
- Histogram uses a fixed reusable box pool.

## 13. Logic Isolation
Changing only visual settings must not intentionally alter:
- confluence score
- confidence
- regime
- conflict flag
- active/reliable engine counts
- stable confluence state

Compare P08.0 outputs against P07.2 under equivalent core/weight settings.

## Acceptance Gate
P08.0 passes when:
1. it compiles and runs cleanly,
2. preset hierarchy is usable,
3. VWAP reset line is visually broken,
4. histogram toggle fully hides/shows the histogram,
5. chart objects remain bounded,
6. diagnostics remain separate from production visuals,
7. no visual toggle changes the intended confluence logic.
