# PLUTUS P04.0 — Value Behavior Test Plan

## Compile gate
- Pine Script v6 compiles without errors.

## Scope/reset inheritance
- Test Asia, London, New York, and Day profile scopes.
- Confirm profile storage resets cleanly at the selected scope boundary.
- Confirm POC / VAH / VAL do not bridge across resets.

## Acceptance
- Verify ACCEPTANCE_ABOVE_VALUE when the configured share of recent closes remains above VAH.
- Verify ACCEPTANCE_BELOW_VALUE when the configured share remains below VAL.
- Verify ACCEPTANCE_INSIDE_VALUE when the configured share remains within VAH/VAL.

## Rejection
- Force/observe trade above VAH followed by re-entry into value; expect REJECTION_FROM_ABOVE_VALUE.
- Force/observe trade below VAL followed by re-entry into value; expect REJECTION_FROM_BELOW_VALUE.
- Vary Rejection Lookback to confirm sensitivity changes logically.

## Failed acceptance
- Establish accepted-above context, then close back below VAH; expect FAILED_ACCEPTANCE_ABOVE.
- Establish accepted-below context, then close back above VAL; expect FAILED_ACCEPTANCE_BELOW.

## Rotation
- Observe repeated POC crossings while price remains accepted inside value.
- Expect ROTATION_AROUND_POC once minimum crossing requirement is satisfied.
- Confirm raising Minimum POC Crosses makes classification stricter.

## Directional auction
- Establish acceptance above VAH with sufficient ATR-normalized distance from POC; expect DIRECTIONAL_AUCTION_HIGHER.
- Establish acceptance below VAL with sufficient negative ATR-normalized distance; expect DIRECTIONAL_AUCTION_LOWER.
- Confirm distance threshold input changes classification logically.

## State behavior
- Confirm live bar reports FORMING.
- Confirm closed bar reports the final categorical state.
- Confirm VALUE_UNAVAILABLE outside closed session scopes or before profile is valid.
- Confirm VALUE_TRANSITION appears when no stronger condition has precedence.

## Diagnostics
- Validate table values for VAH, POC, VAL, acceptance ratios, POC crosses, ATR distance, rejection flags, confirmation status, and state.

## Sanity instruments
- XAUUSD intraday.
- Major FX pair.
- Equity index or CFD.
- Crypto if available.

## Acceptance gate
P04.0 passes when it compiles cleanly, resets correctly, categorical states match visible auction behavior, and no repaint-like post-close state changes are observed.
