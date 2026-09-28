# PLUTUS P02.4 — Unified Institutional Fair-Value State Test Plan

## Objective
Validate that P02.4 consolidates P02.0-P02.3 into a stable, auditable fair-value state engine without changing the validated session/timezone/reset mechanics.

## Compile Gate
- Pine Script v6 compiles with no errors.
- No runtime exceptions on first loaded bar.

## Timing / Reset Checks
1. Confirm fixed sessions remain:
   - Asia 00:00-08:00
   - London 08:00-16:30
   - New York 13:30-20:00
2. Confirm the shared timezone input controls sessions and D/W/M boundaries.
3. Confirm Primary Value VWAP breaks cleanly at its selected reset.
4. Confirm Primary Anchored VWAP breaks cleanly when its anchor is replaced.

## Primary Value Reference Checks
Test each mode:
- Active Session
- Daily
- Weekly
- Monthly

For Active Session, verify priority during overlap:
1. New York
2. London
3. Asia

## Value Migration Checks
Verify diagnostic outputs react plausibly:
- Slope / ATR
- Accepted Above
- Accepted Below
- Equilibrium

Expected directional behavior:
- rising VWAP + accepted above -> bullish value context
- falling VWAP + accepted below -> bearish value context
- repeated crosses near VWAP -> equilibrium
- opposite-side displacement against migration -> migration failure

## Deviation Context Checks
With Band Basis = ATR and then StdDev:
- verify inner/outer levels remain centered on Primary Value VWAP
- price beyond outer upper -> VALUE_EXTENDED_HIGH
- price beyond outer lower -> VALUE_EXTENDED_LOW
- transition against flat/opposing slope can produce VALUE_REVERSION_CONTEXT

## Anchor Checks
Test:
- Latest Swing High
- Latest Swing Low
- Asia Open
- London Open
- New York Open
- Day Open
- Week Open
- Month Open
- Manual Time

For swing anchors:
- anchor only becomes available after pivot confirmation
- cost basis starts from the actual pivot bar
- latest confirmed pivot replaces the prior anchor

## Score Checks
Inspect table values:
- Migration Score
- Acceptance Score
- Directional Score
- Confluence Score

Expected signs:
- bullish alignment -> positive scores
- bearish alignment -> negative scores
- mixed evidence -> compressed/near-zero score

## Unified State Vocabulary
Confirm only the intended states appear:
- FORMING
- VALUE_UNAVAILABLE
- VALUE_CONFLICT
- VALUE_MIGRATION_FAILURE
- VALUE_EQUILIBRIUM
- VALUE_EXTENDED_HIGH
- VALUE_EXTENDED_LOW
- VALUE_REVERSION_CONTEXT
- VALUE_ANCHOR_CONFLUENCE
- VALUE_ACCEPTED_BULLISH
- VALUE_ACCEPTED_BEARISH
- VALUE_MIGRATING_HIGHER
- VALUE_MIGRATING_LOWER
- VALUE_BALANCED

## Precedence Checks
Force or locate examples where multiple conditions coexist and confirm precedence:
1. unavailable
2. conflict
3. migration failure
4. equilibrium
5. extension
6. reversion context
7. neutral anchor confluence
8. accepted directional value
9. value migration
10. balanced value

## Non-Repainting / Confirmation Check
- Historical confirmed bars must not change state after reload under unchanged data.
- Live bar must display FORMING until confirmed.
- Swing AVWAP must not appear before pivot confirmation.

## Visual Cleanliness
Default view should show only:
- Primary Value VWAP
- Primary Anchored VWAP
- diagnostic table

Deviation bands remain optional and OFF by default.
