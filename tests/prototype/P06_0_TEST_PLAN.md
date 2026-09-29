# PLUTUS P06.0 — Divergence Baseline Test Plan

## Objective
Validate confirmed, non-repainting momentum divergence detection using price pivots paired with RSI or MACD Histogram context.

## Compile gate
- Pine Script v6 compiles without errors.
- No array/runtime errors.
- No repainting caused by unconfirmed pivots.

## Pivot confirmation
- Divergence can only be emitted after the right-side pivot confirmation window has completed.
- Historical divergence endpoints must remain fixed after confirmation.
- Changing live-bar price before pivot confirmation must not create a permanent historical divergence.

## Oscillator modes
Test both:
- RSI
- MACD Histogram

Confirm oscillator values shown in diagnostics correspond to the confirmed price pivots.

## Pairing tolerance
- `0` uses oscillator value on the exact price-pivot bar.
- Higher values allow a nearby oscillator extreme around the confirmed price pivot.
- Effective tolerance must never exceed the pivot right-bar count.

## Regular bullish divergence
Expected:
- current confirmed price low < previous confirmed price low
- current oscillator low > previous oscillator low by at least `Minimum Oscillator Separation`
- pivot gap <= `Maximum Pivot Separation`
- price separation >= `Minimum Price Separation / ATR`

Expected state:
`DIVERGENCE_REGULAR_BULLISH`

## Regular bearish divergence
Expected:
- current confirmed price high > previous confirmed price high
- current oscillator high < previous oscillator high by at least the minimum oscillator separation

Expected state:
`DIVERGENCE_REGULAR_BEARISH`

## Hidden bullish divergence
Expected:
- current confirmed price low > previous confirmed price low
- current oscillator low < previous oscillator low by the minimum oscillator separation

Expected state:
`DIVERGENCE_HIDDEN_BULLISH`

## Hidden bearish divergence
Expected:
- current confirmed price high < previous confirmed price high
- current oscillator high > previous oscillator high by the minimum oscillator separation

Expected state:
`DIVERGENCE_HIDDEN_BEARISH`

## Filters
Confirm no event is emitted when:
- pivot separation exceeds the configured maximum
- price separation is below the ATR-normalized minimum
- oscillator separation is below the configured minimum
- the relevant divergence type is disabled

## Score
- Bullish scores must be positive.
- Bearish scores must be negative.
- Score must stay bounded to -100..100.
- Hidden divergence receives a modest weighting discount relative to regular divergence.

## Drawing discipline
- Regular divergence uses solid lines.
- Hidden divergence uses dashed lines.
- Bullish and bearish labels appear at the second pivot only when enabled.
- `Maximum Drawn Events` bounds both line and label arrays.
- Old drawings are deleted cleanly as the limit is exceeded.

## Diagnostics
Verify:
- selected oscillator
- pivot left/right strength
- effective pairing tolerance
- latest confirmed low/high price and oscillator values
- regular/hidden toggles
- divergence score
- confirmation state
- current categorical state

## Cross-asset sanity
Test at minimum on:
- NAS100 / US100
- XAUUSD
- EURUSD
- BTCUSD

Use several intraday timeframes. Confirm divergences remain anchored to the same confirmed pivots after chart refresh.