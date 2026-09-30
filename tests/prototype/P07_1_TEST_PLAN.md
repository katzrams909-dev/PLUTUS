# PLUTUS P07.1 — Confluence Quality / Regime Handling Test Plan

## Objective
Validate that P07.1 improves the P07.0 aggregation model without introducing unstable or misleading confluence states.

## Compile Gate
- Pine Script v6 compiles without errors or warnings that change runtime behavior.
- No unsupported rolling-sum calls such as `ta.sum()`.

## Functional Checks
1. **Engine availability**
   - P03 remains unavailable until enough profile bars exist.
   - P05/P06 do not receive full effective weight before a qualifying zone/divergence exists.
   - Effective weights fall when reliability is low.

2. **Regime classification**
   - `TREND_EXPANSION` requires both ADX trend strength and elevated ATR ratio.
   - `TREND`, `EXPANSION`, `COMPRESSION`, and `BALANCED` appear under appropriate conditions.
   - Regime changes alter effective engine weights but not raw engine scores.

3. **False consensus suppression**
   - Fewer than `Minimum Active Engines` produces `CONFLUENCE_LOW_CONFIDENCE` unless conflict takes priority.
   - Fewer than `Minimum Reliable Engines` also produces low-confidence treatment.
   - Low-magnitude same-direction engines do not create strong confluence.

4. **Conflict handling**
   - Meaningful bullish and bearish evidence on both sides can produce `CONFLUENCE_CONFLICT`.
   - Conflict reduces confidence by the configured penalty.
   - Conflict has priority over directional states.

5. **Persistence / hysteresis**
   - Strong states require the configured consecutive-bar confirmation count.
   - Existing bullish state remains until score falls below the bullish exit threshold.
   - Existing bearish state remains until score rises above the bearish exit threshold.
   - State does not flip repeatedly around the directional entry threshold.

6. **Confidence**
   - Confidence rises with active-engine participation, reliable-engine participation, and directional agreement.
   - Weak consensus and conflict apply separate penalties.
   - Confidence remains clamped to 0–100.

7. **Confluence score**
   - Signed score remains within -100..100.
   - Regime-adjusted effective weights are reflected in the score.
   - Bullish evidence and bearish evidence remain non-negative.

## Visual / Diagnostic Checks
- Diagnostic table can be toggled off.
- Table shows raw score, effective weight, and reliability percentage for P01–P06.
- Regime, ATR ratio, ADX, active/reliable counts, conflict, confidence, strong-state runs, and final state are visible.
- Optional bias label shows state, regime, score, and confidence.
- No entry/exit signal labels are produced.

## Acceptance Gate
P07.1 is accepted when:
- it compiles cleanly,
- regime changes are plausible,
- weak-consensus suppression behaves correctly,
- conflict handling is stable,
- and hysteresis materially reduces bar-to-bar state flipping without trapping stale directional states.
