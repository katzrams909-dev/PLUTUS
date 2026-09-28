# PLUTUS P02.2 — Deviation Bands Test Plan

## Compile gate
- Pine Script v6 compiles without errors.
- No runtime errors when switching symbols or timeframes.

## Baseline preservation
- Asia VWAP uses fixed 00:00-08:00 window.
- London VWAP uses fixed 08:00-16:30 window.
- New York VWAP uses fixed 13:30-20:00 window.
- Daily / Weekly / Monthly VWAPs preserve clean reset breaks.
- Shared timezone behavior matches P02.0/P02.1.

## Reference selection
Test each primary reference:
- Active Session
- Daily
- Weekly
- Monthly

For Active Session verify New York takes priority during London/New York overlap.

## Band basis
Test both:
- ATR
- StdDev

Verify inner and outer bands remain centered on the selected VWAP.

## Reset behavior
At the selected reference reset:
- inner bands break cleanly
- outer bands break cleanly
- no diagonal/vertical bridge is drawn across the reset

## Classification
Verify diagnostic Band State changes appropriately among:
- INNER_VALUE_AREA
- INNER_VALUE_EQUILIBRIUM
- UPPER_TRANSITION
- LOWER_TRANSITION
- UPPER_REVERSION_CONTEXT
- LOWER_REVERSION_CONTEXT
- EXTENDED_ABOVE_OUTER
- EXTENDED_BELOW_OUTER
- UNAVAILABLE

## Visual quality
- Inner/outer bands should not obscure price.
- Inner fill can be disabled independently.
- Existing VWAP lines remain readable.

## Non-signal constraint
P02.2 provides spatial value context only. No buy/sell signals should be generated.
