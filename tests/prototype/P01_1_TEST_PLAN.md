# PLUTUS P01.1 — Effort vs Result Test Plan

## Objective

Validate whether the first interpreted participation states behave sensibly across instruments and timeframes before footprint/order-flow data is introduced.

## Build under test

`pine/prototypes/P01_VolumeIntelligence/PLUTUS_P01_1_EffortResult.pine`

## Required validation sequence

Test at minimum on:

1. FX / tick volume — GBPUSD or EURUSD
2. Futures / centralized traded volume
3. Equity
4. Crypto

Use at least one intraday timeframe and one higher timeframe where practical.

## What to inspect

### Efficient Expansion

Expected characteristics:
- RVOL or abnormal Z-score indicates high effort
- body and total range are large relative to ATR
- close is near the directional extreme
- persistence requirement is satisfied

Reject if expansion repeatedly appears on small indecisive candles.

### Bullish Absorption Candidate

Expected characteristics:
- bearish price effort encounters high participation
- downside result is poor, or price closes strongly away from the low
- repeated condition meets confirmation requirement

Reject if it appears simply because volume is high during ordinary bearish continuation.

### Bearish Absorption Candidate

Expected characteristics:
- bullish price effort encounters high participation
- upside result is poor, or price closes strongly away from the high
- repeated condition meets confirmation requirement

Reject if it appears simply because volume is high during ordinary bullish continuation.

### Liquidity Vacuum

Expected characteristics:
- low normalized effort
- unusually strong directional result
- close near directional extreme

Reject if it fires frequently during normal trend bars.

### Compression

Expected characteristics:
- low normalized effort
- small body and small total range relative to ATR

Reject if it dominates ordinary balanced trading.

## Parameters to stress-test

- Baseline length: 20 / 50
- RVOL low: 0.60 / 0.70 / 0.80
- RVOL high: 1.10 / 1.20 / 1.40
- Body high: 0.60 / 0.80 / 1.00 ATR
- Range high: 0.90 / 1.00 / 1.20 ATR
- Confirmation bars: 1 / 2 / 3

## Acceptance criteria

P01.1 passes only if:

- states are sparse enough to remain meaningful;
- absorption does not behave like a generic reversal detector;
- efficient expansion aligns with visibly strong participation and price progress;
- liquidity-vacuum states remain uncommon;
- compression corresponds to genuinely low-effort, low-result conditions;
- behavior is directionally coherent across different volume types.

Thresholds remain provisional until this validation is complete.
