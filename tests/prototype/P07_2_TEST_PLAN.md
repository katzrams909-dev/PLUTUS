# PLUTUS P07.2 — Unified Confluence State Test Plan

## Objective
Validate the final P07 downstream confluence interface before moving to P08 Visual/UX and P09 Signals.

## Compile Gate
- Pine Script v6 compiles with no errors or warnings that affect execution.
- No `ta.sum()` usage.

## Core Outputs
Confirm the script exposes and updates:
- confluence score in the range -100 to +100
- confidence score in the range 0 to 100
- final direction: bullish / bearish / neutral
- regime state
- active engine count
- reliable engine count
- conflict flag
- strongest supporting engine
- strongest opposing engine
- stable categorical state
- material transition flag

## State Validation
Expected states:
- `CONFLUENCE_STRONGLY_BULLISH`
- `CONFLUENCE_BULLISH`
- `CONFLUENCE_NEUTRAL`
- `CONFLUENCE_BEARISH`
- `CONFLUENCE_STRONGLY_BEARISH`
- `CONFLUENCE_CONFLICT`
- `CONFLUENCE_LOW_CONFIDENCE`
- `FORMING`

Verify that:
- strong states require configured persistence.
- hysteresis prevents excessive directional flipping near threshold.
- conflict overrides directional states when opposing evidence is sufficiently strong.
- low-confidence state appears when active/reliable engine requirements are not met.

## Regime Validation
Test representative trending, expanding, compressed and balanced markets.
Expected regime labels:
- `TREND_EXPANSION`
- `TREND`
- `EXPANSION`
- `COMPRESSION`
- `BALANCED`

Confirm regime changes alter effective engine weights without producing runtime errors.

## Supporting/Opposing Engine Validation
For a bullish state:
- strongest support must be a positive weighted contribution.
- strongest opposing engine must be the largest negative weighted contribution when one exists.

For a bearish state:
- strongest support must be a negative weighted contribution in market direction.
- strongest opposing engine must be the largest positive weighted contribution when one exists.

For a neutral state:
- support field may represent the strongest absolute contribution.
- opposing field may remain `NONE`.

## Transition Validation
A material transition should require a confirmed state change plus at least one of:
- directional reversal/change,
- configured score delta,
- transition into conflict,
- transition into low confidence.

Confirm optional transition labels only appear when enabled.

## Non-Repaint / Confirmation Gate
- Stable state changes only on confirmed bars.
- Transition flag is based on confirmed state behavior.
- Confirmed pivot logic used in the compact P06 representation does not move after confirmation.

## Diagnostic UI
Confirm table displays:
- P01–P06 scores
- effective weights
- reliability percentages
- regime / ATR ratio / ADX
- bull and bear evidence
- active and reliable counts
- conflict / low confidence
- confidence / score / direction
- strongest support / opposition
- strong-state runs
- transition state
- downstream readiness

## Acceptance Criteria
P07.2 is accepted when:
1. it compiles cleanly,
2. state/output ranges are valid,
3. no runtime errors occur on normal chart history,
4. support/opposition reporting is directionally correct,
5. transition detection behaves logically,
6. diagnostics remain stable across symbols/timeframes.

After acceptance, P07 is complete and development moves to P08 — Visual / UX.
