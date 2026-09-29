# P05.1 — Institutional Qualification Integration Test Plan

## Purpose
Validate that P05.1 preserves P05.0 candidate/lifecycle behavior while qualifying zones with ported P01-P04 evidence.

## Compile gate
- Pine Script v6 compiles without errors or warnings that prevent execution.
- Script loads on symbols with standard volume data.
- Invalid or same-timeframe LTF selection must not crash the script; diagnostics should report `INVALID_USE_LOWER_TF` or `LOWER_NO_DATA`.

## Candidate parity
1. Enable FVG only and confirm bullish/bearish FVG candidates are created only when:
   - a three-bar gap exists,
   - the current bar has directional displacement,
   - gap width passes ATR filters.
2. Enable OB only and confirm bullish/bearish OB candidates require:
   - opposite-color prior candle,
   - current displacement,
   - close through the prior candle extreme,
   - width below the maximum ATR threshold.
3. Confirm breaker candidates are only generated from invalidated OBs, not from FVGs or existing breakers.

## P01 participation evidence
- Use a chart timeframe with a strictly lower `Estimated Flow Lower TF`.
- Confirm `LTF Status = VALID` when intrabars are available.
- Confirm bullish high-effort bars generally raise P01 support for bullish candidates and reduce/zero support for bearish candidates; inverse for bearish bars.
- Change LTF to equal/higher than chart timeframe and confirm zone logic still runs using non-flow P01 evidence.

## P02 fair-value evidence
- Compare candidates created above/below the selected VWAP reference.
- Confirm rising/accepted-above VWAP conditions favor bullish P02 support.
- Confirm falling/accepted-below VWAP conditions favor bearish P02 support.
- Test `Active Session`, `Daily`, `Weekly`, and `Monthly` references.

## P03 profile evidence
- Confirm POC/VAH/VAL develop only inside the selected profile scope.
- Confirm profile reset at the next selected session/day boundary.
- Confirm upward POC migration plus price in upper/above value favors bullish P03 support; inverse for bearish.
- Confirm optional POC/VAH/VAL plots match diagnostics.

## P04 auction/value evidence
- Confirm acceptance above VAH contributes positive P04 evidence.
- Confirm acceptance below VAL contributes negative P04 evidence.
- Confirm rejection from above value contributes bearish evidence and rejection from below contributes bullish evidence.
- After at least one completed profile, confirm current-vs-previous value displacement contributes directionally.

## Qualification score
The birth score is bounded 0–100 and uses:
- Mechanics: 20%
- P01 participation: 25%
- P02 fair value: 20%
- P03 profile: 15%
- P04 auction/value: 20%

Checks:
- A candidate with strong same-direction evidence across all engines should score materially higher than one with conflicting context.
- Opposite-direction engine evidence must not add positive support.
- `Show Only Qualified Zones` must hide candidates below the configured threshold.
- Diagnostic `Last Zone Score` must equal the weighted component snapshot shown in the table, subject to rounding.

## Lifecycle
- First revisit changes a fresh zone to mitigated and subtracts the configured mitigation penalty once.
- A bullish zone invalidates on a close below its bottom.
- A bearish zone invalidates on a close above its top.
- Invalidated OBs may create opposite-direction breakers when enabled.
- Breakers are re-qualified using current P01-P04 evidence in the new direction, not by blindly inheriting the original OB score.
- Zones expire after `Maximum Zone Age`.
- With remove-invalidated/expired enabled, their boxes disappear.

## Object discipline
- Set a small `Maximum Tracked Zones` and confirm old zones are pruned without array errors.
- Increase candidate frequency and verify displayed boxes remain bounded by the configured tracking limit and indicator box limit.
- Toggle `Show Candidates` off and confirm analytical tracking still continues without visible boxes.

## Non-signal requirement
P05.1 is context/qualification only. It must not emit buy/sell trade signals.

## Known prototype limitation
P01-P04 evidence is ported into this standalone prototype because Pine indicators cannot directly consume another indicator's local variables. Final integration should expose/reuse shared engine calculations rather than maintain duplicated formulas across independent prototypes.
