# P03.3 — Profile Migration / Developing Value Test Plan

## Objective
Validate that the developing volume-profile migration model correctly interprets changes in POC, VAH, VAL, value-area width, and the relationship between the current profile and the previous completed profile.

## Compile gate
- Pine Script v6 compiles without errors.
- Indicator loads on chart without runtime errors.

## Functional checks
1. Confirm POC / VAH / VAL remain consistent with P03.1 behavior for the same settings.
2. Verify session/day resets follow the selected profile scope and shared ARTHA timezone conventions.
3. During a profile that shifts upward, confirm POC Migration / ATR becomes positive and VAH/VAL migration are directionally consistent.
4. During a profile that shifts downward, confirm POC Migration / ATR becomes negative and VAH/VAL migration are directionally consistent.
5. Verify `VALUE_MIGRATING_HIGHER` and `VALUE_MIGRATING_LOWER` require persistence rather than a single-bar change.
6. Verify value-area width expansion/contraction states react to the configured percentage threshold.
7. Confirm previous completed POC / VAH / VAL are captured at the profile boundary before the new profile begins.
8. Confirm `POC vs Prev / ATR` and `Width vs Prev` compare the active profile with the last completed profile.
9. Confirm the live bar reports `FORMING`; historical confirmed bars report a confirmed migration state.
10. Confirm completed profile plots, when enabled, do not interfere with calculation logic.

## Expected state vocabulary
- `PROFILE_UNAVAILABLE`
- `VALUE_MIGRATING_HIGHER_EXPANDING`
- `VALUE_MIGRATING_LOWER_EXPANDING`
- `VALUE_MIGRATING_HIGHER`
- `VALUE_MIGRATING_LOWER`
- `VALUE_AREA_EXPANDING`
- `VALUE_AREA_CONTRACTING`
- `VALUE_LEANING_HIGHER`
- `VALUE_LEANING_LOWER`
- `PROFILE_BALANCED`
- `PROFILE_TRANSITION`
- live bar: `FORMING`

## Regression checks
- No persistent line-object accumulation.
- No profile leakage across session/day resets.
- No changes to the single-price-per-bar allocation approximation inherited from P03.0–P03.2.
- Bounded storage remains enforced through Maximum Bars Per Profile.
