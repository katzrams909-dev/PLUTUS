# PLUTUS P04.1 — Value Overlap / Separation Test Plan

## Compile gate
- Pine Script v6 compiles without errors or warnings that affect execution.

## Core relationship checks
Test each supported profile scope: Asia, London, New York, Day.

Confirm P04.1 can produce and distinguish:
- `VALUE_OVERLAPPING`
- `VALUE_OVERLAPPING_HIGHER`
- `VALUE_OVERLAPPING_LOWER`
- `VALUE_SEPARATED_HIGHER`
- `VALUE_SEPARATED_LOWER`
- `VALUE_STRONGLY_SEPARATED_HIGHER`
- `VALUE_STRONGLY_SEPARATED_LOWER`
- `VALUE_INSIDE_PREVIOUS`
- `VALUE_ENGULFS_PREVIOUS`
- `VALUE_RELATIONSHIP_TRANSITION`
- `VALUE_RELATIONSHIP_UNAVAILABLE`
- live-bar `FORMING`

## Completed-profile snapshot
- Verify previous VAH / VAL / POC are captured from the profile immediately before reset.
- Verify daily scope captures the prior day, not two days back.
- Verify session scopes capture the immediately prior selected session.
- Confirm previous completed values remain stable while the new profile develops.

## Overlap math
- Overlap % must remain between 0 and 100.
- 100% overlap should occur when the smaller value area is fully contained by the larger one.
- Separated profiles should report 0% overlap.
- Confirm overlap is based on VAH/VAL intervals, not POC distance alone.

## Displacement math
- Positive midpoint shift = current value higher than previous value.
- Negative midpoint shift = current value lower than previous value.
- Displacement Score must remain bounded to [-100, +100].
- Check `Minimum Midpoint Shift / Previous VA Width %` changes overlap-higher/lower sensitivity as expected.

## Separation
- Confirm current VAL >= previous VAH classifies as separated higher.
- Confirm current VAH <= previous VAL classifies as separated lower.
- Confirm strong-separation state activates only when the gap / ATR exceeds the configured threshold.

## Containment states
- Current VA fully inside previous VA => `VALUE_INSIDE_PREVIOUS`.
- Current VA fully contains previous VA => `VALUE_ENGULFS_PREVIOUS`.
- Confirm containment precedence is deterministic and does not misclassify as simple overlap.

## Plot/reset behavior
- Current POC/VAH/VAL must not bridge across profile resets.
- Previous completed levels remain optional and stable.
- Toggling current/previous plots must not alter state calculations.

## Live-bar behavior
- Confirm categorical output is `FORMING` on an unconfirmed live bar.
- Historical/confirmed bars must show the finalized relationship state.

## Sanity markets/timeframes
Where available, test:
- XAUUSD
- major FX pair
- equity/index future or CFD
- crypto

Use at least two intraday timeframes and Day profile mode.
