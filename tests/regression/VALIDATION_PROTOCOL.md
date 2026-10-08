# PLUTUS v1.0 — Validation Protocol

This protocol completes the three release-validation layers for the frozen v1.0 model.

Frozen trading-model baseline:
`c2e8d0860164f98b372038f177a92c0bf9d0bfae`

Current presentation/validation build:
`pine/release/PLUTUS_RC1.pine`

The validation telemetry is diagnostic only and must not feed signal generation.

---

## 1. Reload Determinism

### Purpose
Verify that confirmed historical signals are reproducible after reload and are not being repositioned by later information.

### Validation key
The Validation panel exposes a deterministic last-signal key in the form:

`time | L/S | zoneId | reactionClass | regime | SQ`

Example:

`1791417600000|S|741|BREAKER_RETEST|REVERSAL|SQ72.4`

### Procedure
For each selected market/timeframe:

1. Enable **Release Validation → Show Validation Panel**.
2. Record at least 20 historical confirmed signals.
3. For each signal record:
   - timestamp
   - LONG/SHORT
   - zone ID
   - reaction class
   - regime
   - SQ
   - entry / stop / target
4. Reload the TradingView chart.
5. Compare the same historical area.

### PASS
- same signal candle
- same direction
- same zone ID
- same reaction class
- same regime
- same SQ within displayed rounding
- same entry / stop / target
- no historical label shifts backward

### FAIL / P0
Any historical signal materially changes after reload without settings/data changing.

---

## 2. Pending-Reaction Synchronization

### Purpose
Validate the multi-zone 1–3 bar reaction inventory.

The Validation panel shows:

`Pending sync: PASS · N active`

### Runtime checks performed
- all pending arrays have identical sizes
- duplicate primary-zone/cluster-pair keys are detected
- pending count remains bounded
- completed pending thesis is removed after confirmation
- invalid/expired pending thesis is removed

### Stress procedure

Use Diagnostic or Full display and test:
- max active zones
- volatile sessions
- overlapping FVG + OB / Breaker structures
- both bullish and bearish interactions occurring close together
- 1B / 2B / 3B reactions
- wick-only interactions
- zone invalidation during pending state
- IFVG / Breaker flip followed by later retest

### PASS
- Pending sync always shows PASS
- pending inventory returns toward zero after interactions resolve
- no duplicate signals from one pending thesis
- no signal from deleted/retired inventory
- cluster loses eligibility if a required member is no longer live

### FAIL / P0-P1
- Pending sync reports FAIL
- array/runtime error
- duplicated pending pair
- orphaned pending setup
- wrong zone provenance after array removal

---

## 3. Objective Signal Statistics

### Purpose
Evaluate whether signal classes and SQ correspond to useful forward outcomes without converting the production indicator into a strategy.

The Validation panel tracks completed signals using structural risk `R`.

### Metrics
- total confirmed signals
- Trend / Countertrend / Reversal counts
- 1B / 2B / 3B counts
- wick-confirmed count
- percentage reaching 1R
- percentage reaching 2R
- average MFE in R
- average MAE in R
- ambiguous bars

### Outcome horizon
Controlled by:

`Release Validation → Outcome Evaluation Horizon (Bars)`

Default: 100 bars.

A tracked outcome closes when:
- price reaches 2R,
- structural stop reaches -1R,
- or the evaluation horizon expires.

### Ambiguous bar
An ambiguous bar is one in which OHLC data shows both a favorable threshold and the stop threshold could have been reached in the same candle. Intrabar order is unknown, so the event is counted separately rather than assumed.

### Recommended sample
Minimum useful first pass:
- 100+ completed signals total
- at least 20 Trend
- at least 20 Countertrend
- at least 20 Reversal where available

### Breakdown to record manually
For serious evaluation, record performance by:
- reaction class
- regime
- instrument
- timeframe
- session
- SQ bucket:
  - <55
  - 55–64
  - 65–74
  - 75+

### Interpretation
The objective is not to maximize raw win rate.

Useful evidence would be:
- higher SQ buckets show better MFE / 1R / 2R behavior
- Trend / Countertrend / Reversal behave differently in explainable ways
- no single reaction class produces a disproportionate concentration of poor outcomes
- MAE remains consistent with the chosen structural stop logic

---

## Required Release Matrix

| Market | TF | Determinism | Pending Sync | Statistics |
|---|---:|---|---|---|
| XAUUSD | 15m | [ ] | [ ] | [ ] |
| XAUUSD | 1h | [ ] | [ ] | [ ] |
| NAS100 | 5m | [ ] | [ ] | [ ] |
| NAS100 | 15m | [ ] | [ ] | [ ] |
| SPX500 | 5m | [ ] | [ ] | [ ] |
| SPX500 | 15m | [ ] | [ ] | [ ] |
| GBPUSD | 15m | [ ] | [ ] | [ ] |
| AUDUSD | 1h | [ ] | [ ] | [ ] |
| USDCAD | 15m | [ ] | [ ] | [ ] |

---

## Release rule

A trading-model change is not justified by one losing trade or one missed signal.

Only reopen frozen logic for:
- deterministic failure
- wrong direction
- lifecycle corruption
- invalid provenance
- pending-state synchronization error
- reproducible systematic defect across multiple examples
