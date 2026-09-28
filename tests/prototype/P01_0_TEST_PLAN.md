# PLUTUS P01.0 Test Plan

## Objective

Validate baseline volume normalization before adding absorption, exhaustion, footprint, or signal logic.

## Build under test

`pine/prototypes/P01_VolumeIntelligence/PLUTUS_P01_0_VolumeIntelligence.pine`

## Required checks

### 1. Compilation
- Pine Script v6 compiles without warnings/errors that affect execution.
- Diagnostic table renders correctly.

### 2. Instrument coverage
Test representative symbols from each category where available:
- equity
- index future
- commodity future
- crypto
- spot forex / CFD

Record `syminfo.volumetype` shown by the diagnostic table.

### 3. Timeframe coverage
Test at minimum:
- 5m
- 15m
- 1H
- 4H
- 1D

### 4. RVOL behavior
Confirm:
- quiet bars commonly classify as contraction
- average participation clusters around normal
- visibly elevated volume reaches expansion
- only unusually elevated bars reach climax candidate

Do not tune thresholds from one symbol alone.

### 5. Z-score behavior
Compare Z-score with RVOL during:
- volume spikes
- sustained elevated-volume trends
- low-volume sessions
- regime changes

Goal: determine whether Z-score adds stable information beyond RVOL.

### 6. Effort / Result behavior
Inspect examples of:
- high effort + high result
- high effort + low result
- low effort + high result
- low effort + low result

P01.0 classifications are diagnostic quadrants only. They must not yet be interpreted as confirmed absorption, exhaustion, or liquidity-vacuum signals.

### 7. Data quality
Check symbols with:
- missing volume
- tick volume
- base/quote volume where reported

PLUTUS must display unavailable states cleanly when usable volume is absent.

## Acceptance criteria

P01.0 is accepted when:
1. It compiles and renders reliably.
2. RVOL behaves consistently across tested symbols/timeframes.
3. Volume type is correctly exposed.
4. No obvious numerical instability occurs from zero/NA baselines or standard deviation.
5. The diagnostic table provides enough information to design P01.1 Effort vs Result without adding chart clutter.

## Results

Status: NOT YET VALIDATED ON TRADINGVIEW

Record observations here before P01.1 thresholds are locked.
