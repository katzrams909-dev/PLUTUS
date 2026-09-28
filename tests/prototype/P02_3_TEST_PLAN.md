# PLUTUS P02.3 — Anchored VWAP Test Plan

## Compile gate
- Pine Script v6 compiles without errors.
- No runtime errors on common intraday symbols/timeframes.

## Anchor-mode checks
Test each primary anchor mode:
- Latest Swing High
- Latest Swing Low
- Asia Open
- London Open
- New York Open
- Day Open
- Week Open
- Month Open
- Manual Time

For every mode verify:
1. Only one primary AVWAP is active.
2. The line restarts cleanly when a new anchor replaces the prior anchor.
3. No vertical/diagonal bridge is drawn across anchor resets.
4. Diagnostic table reports the expected anchor type and active state.

## Swing-anchor non-repainting checks
- Use pivot left/right values of 3/3 initially.
- Confirm swing-high AVWAP appears only after the pivot-right confirmation delay.
- Confirm swing-low AVWAP appears only after the pivot-right confirmation delay.
- Confirm the cost basis is calculated from the actual pivot bar rather than the confirmation bar.
- Verify historical confirmed anchors do not move after confirmation.

## Fixed-session anchor checks
Using the shared ARTHA timezone model:
- Asia anchor resets at 00:00.
- London anchor resets at 08:00.
- New York anchor resets at 13:30.
- Each anchor persists until the next corresponding session open.

## Calendar anchor checks
- Day Open resets at the selected timezone's new day.
- Week Open resets at the selected timezone's new week.
- Month Open resets at the selected timezone's new month.
- Confirm the selected timezone changes these boundaries consistently.

## Manual-anchor checks
- Set Manual Anchor Time to a visible historical timestamp.
- Confirm AVWAP begins from the first chart bar at/after that timestamp.
- Confirm the line remains stable afterward.

## Research swing-line checks
With optional Swing High / Swing Low AVWAPs enabled:
- Only the latest confirmed high anchor is retained.
- Only the latest confirmed low anchor is retained.
- New confirmed pivots replace prior anchors rather than accumulating objects/lines.

## Context checks
- Price-vs-anchor status must match visual position relative to the AVWAP.
- Distance / ATR sign must be positive above AVWAP and negative below AVWAP.

## Regression checks
- Baseline Asia/London/New York VWAPs still use the fixed ARTHA sessions.
- D/W/M VWAPs still reset in the shared timezone.
- Baseline plot gaps remain clean at resets.

## Acceptance criteria
P02.3 passes when all anchor modes compile and behave deterministically, swing anchors are confirmation-safe, anchor replacement is clean, and no unbounded line/object accumulation occurs.
