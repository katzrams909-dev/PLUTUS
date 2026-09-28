# PLUTUS P02.1 — VWAP Slope & Value Migration Test Plan

## Compile gate
- Pine Script v6 compiles without errors.

## Baseline regression
- Asia VWAP uses fixed 00:00–08:00 session.
- London VWAP uses fixed 08:00–16:30 session.
- New York VWAP uses fixed 13:30–20:00 session.
- Shared ARTHA timezone selector remains functional.
- Session VWAPs terminate at session end and restart on next session.
- Daily / Weekly / Monthly VWAPs reset cleanly without vertical joins.

## Value reference
Test all Primary Value Reference modes:
- Active Session
- Daily
- Weekly
- Monthly

During London/New York overlap, Active Session should resolve to New York.
Outside all fixed sessions, Active Session may report UNAVAILABLE.

## Slope classification
- Rising when slope/ATR meets positive threshold.
- Falling when slope/ATR meets negative threshold.
- Flat when inside threshold band.
- Check behavior around period/session resets for false slope carryover.

## Acceptance
- Accepted Above requires configured proportion of closes above reference.
- Accepted Below requires configured proportion of closes below reference.
- Verify ratios shown in diagnostics match visible price location.

## Equilibrium
- Repeated VWAP crossings within configured ATR distance should classify EQUILIBRIUM.
- One isolated crossing should not classify equilibrium.

## Extension
- Price sufficiently above reference should classify EXTENDED_ABOVE_VALUE.
- Price sufficiently below reference should classify EXTENDED_BELOW_VALUE.

## Migration failure
- Rising VWAP with price displaced below reference should classify RISING_VALUE_FAILURE.
- Falling VWAP with price displaced above reference should classify FALLING_VALUE_FAILURE.

## Non-repainting / confirmation
- Historical states must remain stable.
- Live bar should display FORMING until confirmed.

## Visual check
- Default chart remains readable.
- State markers are OFF by default.
- Diagnostics explain why the current state was selected.
