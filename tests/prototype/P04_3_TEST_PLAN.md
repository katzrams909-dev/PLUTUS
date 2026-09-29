# PLUTUS P04.3 — Unified Value Analysis Test Plan

## Objective
Validate that P04.3 consolidates P04.0-P04.2 without breaking profile mechanics and produces stable, bounded, downstream-ready value-analysis outputs.

## Compile gate
- Pine Script v6 compiles with no errors or warnings that affect execution.
- Indicator loads on chart with default inputs.

## Profile parity
- Current POC / VAH / VAL remain consistent with prior P04 prototypes under the same inputs.
- Session/day reset behavior remains clean.
- Previous completed POC / VAH / VAL snapshot correctly on scope reset.

## P04.0 behavior parity
Confirm diagnostics can still produce:
- ACCEPTANCE_ABOVE_VALUE
- ACCEPTANCE_BELOW_VALUE
- ACCEPTANCE_INSIDE_VALUE
- REJECTION_FROM_ABOVE_VALUE
- REJECTION_FROM_BELOW_VALUE
- FAILED_ACCEPTANCE_ABOVE
- FAILED_ACCEPTANCE_BELOW
- ROTATION_AROUND_POC
- VALUE_TRANSITION

## P04.1 relationship parity
Confirm diagnostics can still produce:
- VALUE_OVERLAPPING
- VALUE_OVERLAPPING_HIGHER
- VALUE_OVERLAPPING_LOWER
- VALUE_SEPARATED_HIGHER
- VALUE_SEPARATED_LOWER
- VALUE_STRONGLY_SEPARATED_HIGHER
- VALUE_STRONGLY_SEPARATED_LOWER
- VALUE_INSIDE_PREVIOUS
- VALUE_ENGULFS_PREVIOUS
- VALUE_RELATIONSHIP_TRANSITION

Check:
- overlap percentage stays in 0-100
- displacement stays in -100 to +100
- separation gap is non-negative

## Score bounds
Verify on multiple symbols/timeframes:
- Signed Acceptance: -100 to +100
- Rejection Strength: -100 to +100
- Rotation Strength: 0 to 100
- Auction Imbalance Score: -100 to +100
- Balance Score: 0 to 100
- Value Analysis Score: -100 to +100
- Confidence Score: 0 to 100

## Unified-state checks
Observe whether the engine can transition among:
- VALUE_STRONGLY_BULLISH
- VALUE_STRONGLY_BEARISH
- VALUE_BULLISH
- VALUE_BEARISH
- VALUE_REJECTION_BULLISH
- VALUE_REJECTION_BEARISH
- VALUE_FAILURE_BULLISH
- VALUE_FAILURE_BEARISH
- VALUE_BALANCED
- VALUE_EXPANDING
- VALUE_ANALYSIS_CONFLICT
- VALUE_ANALYSIS_TRANSITION
- VALUE_ANALYSIS_UNAVAILABLE
- FORMING

## Conflict logic
Find cases where auction direction and inter-profile displacement oppose one another with both magnitudes above the configured conflict threshold. Confirm state becomes VALUE_ANALYSIS_CONFLICT.

## Confirmation discipline
- Historical categorical outputs should use confirmed-bar logic.
- Current live bar should display FORMING until confirmed.

## Visual discipline
- Current POC / VAH / VAL lines remain optional.
- Previous completed levels remain off by default.
- No label or line-object accumulation.
- Diagnostic table can be disabled without affecting analytical logic.

## Performance
Test with:
- 8 bins
- 24 bins default
- 80 bins
- Maximum Bars Per Profile at 1000

Ensure execution remains usable and no object-limit errors occur.

## Scope matrix
Test at minimum:
- Asia
- London
- New York
- Day

Use more than one liquid instrument and more than one intraday timeframe.

## Acceptance gate
P04.3 passes when it compiles cleanly, preserves prior P04 behavior/relationship outputs, keeps all scores bounded, resets correctly, and produces plausible unified states without visual/object leakage.
