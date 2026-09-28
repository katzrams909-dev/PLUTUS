# PLUTUS P03.1 — POC / VAH / VAL Test Plan

## Compile gate
- Pine Script v6 compiles without errors or warnings that indicate invalid state handling.

## Scope/reset checks
Test each Profile Scope: Asia, London, New York, Day.
- Fixed sessions use the shared ARTHA timezone convention.
- Developing levels only plot while a session scope is active.
- Day scope resets on the selected timezone calendar day.
- No vertical/diagonal bridging at profile reset boundaries.

## Profile geometry
- Profile High/Low track the stored bars for the active profile.
- Bin Size remains positive and respects `syminfo.mintick`.
- POC remains inside Profile High/Low.
- VAH >= POC >= VAL.
- VAH and VAL remain inside or on the profile range subject to bin-edge rounding.

## Value-area algorithm
Default target = 70%.
- Expansion starts from the POC bin.
- The next adjacent bin with greater volume is added first.
- Equal adjacent volumes expand both sides when both exist.
- Expansion stops after accumulated value-area volume meets/exceeds target.
- Diagnostic `Value Area Achieved` is >= target except when data is unavailable.

## Price-location classification
Verify:
- close > VAH => ABOVE_VALUE
- POC < close <= VAH => UPPER_VALUE
- close == POC approximately => AT_POC where exact series equality occurs
- VAL <= close < POC => LOWER_VALUE
- close < VAL => BELOW_VALUE

## Completed-profile carryover
Enable Previous Completed POC / VAH / VAL.
- On a new scope, prior developing POC/VAH/VAL are snapshotted.
- Previous levels remain stable until the next profile completes/resets.
- No stale current-profile value is substituted after reset.

## Stress/performance
- Test bins: 8, 24, 50, 80.
- Test max bars: 25, 300, 1000.
- Verify no array-size/runtime errors and acceptable chart responsiveness.

## Known approximation
Each chart bar contributes its full volume to one price bin using the selected allocation price. P03.1 validates auction-value extraction from that baseline distribution; intrabar volume distribution refinement is a later concern if justified.
