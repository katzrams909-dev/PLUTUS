# PLUTUS P03.0 — Volume Profile Baseline Test Plan

## Purpose
Validate the baseline price-bin volume profile engine before adding value-area, HVN/LVN, migration, or unified profile states.

## Compile gate
- Pine Script v6 compiles without errors.

## Timing / scope
Test each `Profile Scope`:
- Asia
- London
- New York
- Day

Confirm the fixed ARTHA session windows and selected timezone behave consistently with P02.

## Reset behavior
For Asia/London/New York:
- profile storage clears at the next selected session start;
- POC/profile range plots appear only while that selected session is active;
- completed session lines do not extend indefinitely;
- no vertical/diagonal line bridges across session resets.

For Day:
- profile resets once at the new local calendar day in the selected timezone;
- no bridge between daily profiles.

## Profile mechanics
During an active profile:
- `Stored Bars` increases as bars are added;
- `Stored Bars` never exceeds `Maximum Bars Per Profile`;
- `Profile High` tracks the highest high of stored bars;
- `Profile Low` tracks the lowest low of stored bars;
- `Bin Size` adjusts as the developing range changes;
- POC remains within Profile High / Profile Low;
- Profile Volume increases with accumulated reported volume.

## Dynamic range remapping
Observe a profile while a new session/day high or low expands the range.
Expected:
- all stored bars are remapped against the new bin geometry;
- the POC may move because bins have changed;
- no stale-bin artifacts remain from the old range.

## POC sanity
- POC should generally migrate toward the most heavily traded allocation-price area.
- `POC Volume %` must remain between 0% and 100%.
- Increasing `Price Bins` should create finer price resolution, not alter total accumulated profile volume.

## Volume-unavailable symbols
On a symbol with unavailable volume:
- table should report `UNAVAILABLE`;
- no runtime error should occur;
- profile should remain empty / N/A.

## Performance
Test at minimum:
- 24 bins / 300 bars
- 48 bins / 500 bars
- 80 bins / 1000 bars

Confirm chart responsiveness remains acceptable. P03.0 intentionally uses no histogram boxes or persistent drawing objects.

## Known approximation
Each chart bar assigns all of its volume to one bin using the selected `Allocation Price` (default HLC3). This is deliberate for P03.0. More granular distribution is a later refinement and should not be assumed to be true price-level traded volume.

## Pass criteria
P03.0 passes when:
1. it compiles cleanly;
2. selected scopes reset correctly;
3. POC/range terminate cleanly;
4. developing range remapping behaves correctly;
5. no runaway objects or material chart slowdown occur.
