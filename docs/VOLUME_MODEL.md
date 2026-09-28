# PLUTUS Volume Intelligence Model

## Purpose

P01 defines the volume and participation primitives that every later PLUTUS engine will consume.

PLUTUS must distinguish between simple activity and meaningful participation. Raw volume alone is insufficient. The engine therefore normalizes volume, compares effort with price result, incorporates footprint-derived buy/sell pressure when available, and emits a compact internal participation state.

## Data hierarchy

PLUTUS will use a two-tier volume model.

### Tier A — universal core

Available on symbols that provide usable `volume` data:

- raw volume
- moving average / EMA of volume
- relative volume (RVOL)
- rolling percentile / z-score style normalization
- price range and true range
- candle body and close location
- directional price change
- volume expansion and contraction
- effort-versus-result relationships

This tier must remain functional when footprint data is unavailable.

### Tier B — footprint enhancement

When `request.footprint()` is available, PLUTUS can additionally consume:

- buy volume
- sell volume
- total volume
- volume delta
- delta percentage
- bar POC row
- bar VAH / VAL rows
- row-level volume
- row-level delta
- buy/sell imbalance flags

Footprint data is an enhancement layer, not a hard dependency for the universal core.

## Volume validity

The engine must inspect volume availability and type before classifying participation.

Relevant symbol metadata:

- `syminfo.type`
- `syminfo.volumetype`

Possible `syminfo.volumetype` values include base, quote, tick, and n/a.

PLUTUS must never imply centralized traded volume where only tick volume exists. Diagnostic output should expose the reported volume type.

If volume is missing or unusable, volume-dependent classifications should return an explicit unavailable state rather than silently substituting price-only logic.

## Core normalized measurements

### 1. Relative Volume

Primary normalized activity measure:

`RVOL = current volume / baseline volume`

Baseline should be configurable between SMA and EMA initially.

Initial default baseline length: 20 bars.

Suggested interpretation bands for prototype testing only:

- RVOL < 0.70: contraction
- 0.70–1.20: normal
- 1.20–1.80: expansion
- >= 1.80: exceptional / climax candidate

These are prototype defaults and must not be treated as universal constants.

### 2. Volume Z-score

Use a rolling standardized volume measure to identify statistically abnormal participation independent of absolute instrument volume.

`Zvol = (volume - mean(volume, N)) / stdev(volume, N)`

Initial default N: 50.

Candidate interpretations:

- below -1: unusually low participation
- -1 to +1: broadly normal
- +1 to +2: elevated
- above +2: abnormal

### 3. Volume percentile

Percentile / rank-based normalization should be evaluated as an alternative or complement to z-score because volume distributions are often skewed.

The prototype should expose both RVOL and one abnormal-volume statistic so we can compare stability across assets.

## Effort vs Result

This is a central P01 concept.

Effort = normalized volume / order-flow participation.
Result = normalized price movement.

Candidate result measures:

- true range relative to ATR
- candle body relative to ATR
- close-to-close displacement relative to ATR

Primary prototype measure:

`Result = abs(close - open) / ATR`

Secondary diagnostic measure:

`RangeResult = true range / ATR`

### Initial effort/result interpretations

High effort + high result:
- efficient expansion
- participation supports movement

High effort + low result:
- absorption candidate
- two-sided trade / opposing inventory likely

Low effort + high result:
- liquidity vacuum / thin participation candidate

Low effort + low result:
- inactivity / compression

The engine should not infer institutional intent from one bar alone. Absorption and exhaustion states should require context and persistence rules.

## Footprint-derived metrics

When footprint data is available:

### Delta

`Delta = buy volume - sell volume`

### Delta percentage

`DeltaPct = Delta / total volume`

This normalizes directional order-flow imbalance across instruments and bars.

### Delta efficiency

Compare directional order flow with price result.

Examples:

- strong positive delta + bullish displacement = efficient buying
- strong positive delta + weak/negative price result = possible buy absorption
- strong negative delta + bearish displacement = efficient selling
- strong negative delta + weak/positive price result = possible sell absorption

### Footprint imbalance density

Count buy and sell imbalance rows and normalize by total footprint rows.

This can later produce:

- buy imbalance density
- sell imbalance density
- dominant side
- stacked imbalance candidate

P01 should calculate the data but avoid overloading the chart with row-level graphics.

## Proposed P01 states

The P01 engine should emit one primary participation state and supporting flags.

Primary states:

- `VOL_UNAVAILABLE`
- `VOL_CONTRACTION`
- `VOL_NORMAL`
- `VOL_EXPANSION`
- `VOL_CLIMAX`
- `VOL_ABSORPTION_BULLISH`
- `VOL_ABSORPTION_BEARISH`
- `VOL_EXHAUSTION_BULLISH`
- `VOL_EXHAUSTION_BEARISH`
- `VOL_LIQUIDITY_VACUUM`

The exact thresholds and precedence order will be validated experimentally.

## Directional semantics

The terms bullish/bearish in absorption states should describe the expected beneficiary of absorption, not the aggressive side being absorbed.

Example:

- strong sell pressure with weak downward result can be classified as bullish absorption
- strong buy pressure with weak upward result can be classified as bearish absorption

This convention must remain consistent throughout PLUTUS.

## State precedence

Initial precedence proposal:

1. unavailable
2. absorption
3. climax / exhaustion
4. liquidity vacuum
5. expansion
6. contraction
7. normal

The purpose is to prevent a generic expansion flag from masking a more informative condition such as absorption.

## Non-repainting rules

Historical P01 states must be based on confirmed bars.

Realtime diagnostics may display developing values, but confirmed state outputs used by downstream engines and alerts must finalize only on bar close unless a later dedicated intrabar mode is explicitly introduced.

No future-bar pivots or lookahead behavior are required by the core P01 engine.

## Performance rules

- use one `request.footprint()` call maximum
- avoid row iteration unless footprint features are enabled
- limit expensive footprint row processing to the metrics actually consumed downstream
- do not create per-row drawing objects in the production P01 engine
- profile before integration with later engines

## P01 prototype stages

### P01.0 — Baseline volume normalization

Implement:
- volume validity
- volume type diagnostics
- RVOL
- abnormal-volume normalization
- ATR-normalized result metrics
- expansion / contraction / climax candidates

### P01.1 — Effort vs Result

Implement:
- efficient expansion
- absorption candidates
- low-effort displacement / liquidity-vacuum candidates
- diagnostic table

### P01.2 — Footprint enhancement

Implement when supported:
- buy/sell volume
- delta and delta percentage
- POC / VA row values
- imbalance density
- footprint-enhanced absorption classification

### P01.3 — State engine

Consolidate metrics into stable enumerated participation states with configurable thresholds and confirmed-bar output.

## Output contract for downstream engines

P01 should ultimately expose at least:

- volumeAvailable
- volumeType
- rvol
- volumeAbnormality
- resultEfficiency
- buyVolume
- sellVolume
- delta
- deltaPct
- buyImbalanceDensity
- sellImbalanceDensity
- participationState
- participationScore
- bullishAbsorption
- bearishAbsorption
- expansion
- contraction
- climax

This output contract will feed VWAP, Value Analysis, Institutional Zones, Divergence, and Confluence engines.

## Open research questions

1. RVOL baseline: SMA vs EMA vs session-normalized baseline.
2. Z-score vs percentile rank for abnormal volume.
3. Best normalized price-result denominator: ATR, median true range, or rolling percentile.
4. Whether footprint delta should override or only enhance candle-direction volume inference.
5. How many consecutive bars should confirm absorption/exhaustion.
6. Whether thresholds should vary automatically by instrument type and volume type.
7. How to degrade gracefully when footprint access is unavailable.
