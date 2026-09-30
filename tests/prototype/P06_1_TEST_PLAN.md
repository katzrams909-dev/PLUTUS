# PLUTUS P06.1 — Divergence Quality / Confirmation Test Plan

## Objective
Validate that P06.1 preserves the confirmed/non-repainting divergence mechanics from P06.0 while filtering weak events with swing quality, oscillator extremity, participation, value context, duplicate suppression and expiry.

## Compile gate
- Pine Script v6 compiles without errors.
- No runtime errors from pivot indexing, arrays, labels or lines.
- No object-limit growth beyond configured bounds.

## Confirmed pivot behavior
- Divergence is evaluated only after `rightBars` confirms the price pivot.
- No divergence event appears before the confirming bar.
- Historical divergence does not move after confirmation.

## Pivot separation
- Pairs closer than `Minimum Pivot Separation` are rejected.
- Pairs beyond `Maximum Pivot Separation` are rejected.
- Price separation must meet `Minimum Price Separation / ATR`.

## Pivot prominence
- Weak local pivots below `Minimum Pivot Prominence / ATR` are rejected.
- More prominent swings produce higher prominence contribution to quality.

## Oscillator pairing
Test both:
- RSI
- MACD Histogram

Confirm local oscillator pairing tolerance remains bounded around the confirmed price pivot.

## Divergence definitions
Regular bullish:
- price lower low
- oscillator higher low

Hidden bullish:
- price higher low
- oscillator lower low

Regular bearish:
- price higher high
- oscillator lower high

Hidden bearish:
- price lower high
- oscillator higher high

## Oscillator extremity
RSI:
- bullish low pivots closer to/below the low extremity score higher
- bearish high pivots closer to/above the high extremity score higher

MACD Histogram:
- larger normalized histogram extremes score higher than near-zero pivots

## Participation confirmation
With `Use Participation Confirmation = ON`:
- RVOL and candle efficiency contribute to quality
- missing/weak participation must not create out-of-range values

With it OFF:
- divergence mechanics continue to work without participation gating

## VWAP/value context
With `Use Daily VWAP Context = ON`:
- bullish divergence receives more context support when the pivot is below daily VWAP
- bearish divergence receives more context support when the pivot is above daily VWAP
- reset behavior follows the exchange-local day

With it OFF:
- divergence mechanics continue without value-context dependence

## Quality threshold
- quality remains in [0,100]
- events below `Minimum Divergence Quality` are not confirmed or drawn
- qualified events include quality in their label

## Duplicate suppression
- same-direction qualified events inside `Duplicate Cooldown` are suppressed
- events after the cooldown may qualify normally
- opposite-direction events are not blocked by the other side's cooldown

## Event expiry
- confirmed state persists only for `Divergence Event Expiry` bars
- after expiry state returns to `DIVERGENCE_NONE`
- score and quality return to zero after expiry
- historical drawing remains bounded for auditability

## Score/state
- bullish score is positive
- bearish score is negative
- score remains within [-100,100]
- hidden divergence uses the configurable hidden weight
- unconfirmed realtime bar reports `FORMING`

## Drawing discipline
- regular divergence uses solid lines
- hidden divergence uses dashed lines
- `Maximum Drawn Events` bounds both line and label arrays
- toggling lines/labels does not affect divergence calculations

## Cross-asset sanity
Test at minimum:
- NAS100
- XAUUSD
- one FX pair
- one crypto pair

Use multiple intraday timeframes and verify the quality filter reduces noisy divergence clusters without eliminating obvious major divergences.
