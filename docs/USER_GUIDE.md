# PLUTUS v1.0 — User Guide

## 1. What PLUTUS Does

PLUTUS is an institutional participation and reaction indicator.

It is designed to answer:

> Where is meaningful capital participating, and is that participation supporting or opposing the current price move?

PLUTUS combines:
- relative volume / participation
- effort-versus-result / absorption
- VWAP and value context
- volume profile
- FVG / IFVG
- Order Blocks / Breaker Blocks
- divergence
- ADX / DI regime
- liquidity / reward space
- Trend / Countertrend / Reversal classification

PLUTUS is not intended to treat every zone touch as a trade.

A zone begins an investigation. The reaction and context decide whether a signal is allowed.

---

## 2. Signal Labels

Default label detail is **Compact**.

### Compact example
`REV SHORT · BB #741 · SQ 72`

Meaning:
- `REV` = Reversal
- `SHORT` = signal direction
- `BB #741` = Breaker Block source and stable zone ID
- `SQ 72` = final Signal Quality

### Detailed example
`REV SHORT · BB RT · BB #741 · 2B W · SQ 72 · 1.8R`

Additional fields:
- `BB RT` = Breaker retest
- `2B` = confirmed on the second bar of the reaction window
- `W` = wick-only interaction occurred
- `1.8R` = available reward space at confirmation

### Regime abbreviations
- `TRD` — Trend
- `CTR` — Countertrend
- `REV` — Reversal

### Reaction abbreviations
- `FVG REJ` — FVG rejection
- `FVG SWP` — FVG sweep/reclaim
- `IFVG RT` — IFVG retest
- `OB REJ` — Order Block rejection
- `OB SWP` — Order Block sweep/reclaim
- `BB RT` — Breaker retest
- `FAIL` — zone failure
- `CL REJ` — cluster rejection
- `CL SWP` — cluster sweep/reclaim

---

## 3. Signal Colors

Under **Signal Visuals**:

- `LONG Signal Color`
- `SHORT Signal Color`

These control direction-sensitive signal objects:
- signal label
- entry line
- target line
- target/reward box
- entry/target price labels

The stop line and risk object remain neutral so LONG/SHORT color meaning stays clear.

---

## 4. Reaction Timing

Under **Zone Reaction Timing**:

### Reaction Confirmation Window
Options: 1–3 bars.

Default: 3.

A 3-bar window means the signal may confirm on bar 1, 2, or 3. PLUTUS uses the earliest valid confirmation.

### Example 2-bar bullish reaction
1. Price enters a Bull OB.
2. First candle closes inside the zone.
3. Next candle closes back out in the bullish direction.
4. Signal can confirm on the second candle.

### Wick reaction
A wick is a valid zone interaction.

Example:
- wick enters Bull OB
- body remains above
- candle closes back above the zone
- reaction may qualify without the body entering the zone

---

## 5. IFVG and Breaker Rule

IFVG and Breaker zones cannot signal on the candle that creates the flip.

Required sequence:

`source failure → flipped zone created → later retest → valid reaction → signal`

The flip candle is never counted as the retest.

---

## 6. Trend / Countertrend / Reversal

Zone type does not determine trade regime.

### Trend
Signal direction agrees with the established regime.

### Countertrend
Signal opposes the current regime but has sufficient location, reaction, absorption, divergence, or liquidity evidence.

### Reversal
Evidence indicates the previous regime is failing and the opposite direction is taking control.

Example:

`bull market → Bear OB forms near high → price sells away → price retests Bear OB → bearish reaction → REV SHORT`

Later bearish setups after regime transition may be classified as Trend shorts.

---

## 7. Signal Quality — SQ

SQ measures the quality of the actual setup.

Components include:
- source quality
- reaction quality
- participation / flow
- regime compatibility
- context / confidence
- reward space

A high SQ does not override a hard invalidation.

For example:
- invalid zone
- insufficient reward space
- severe context contradiction
- failed reaction
- prohibited session
- invalid lifecycle

can still block a signal regardless of SQ.

---

## 8. Volume

PLUTUS uses relative volume and effort-versus-result.

It evaluates:
- RVOL
- arrival pressure
- reaction pressure
- absorption
- directional departure

For FX/CFD instruments, volume is typically broker/feed-dependent tick volume. PLUTUS therefore treats volume as relative participation, not centralized exchange volume.

---

## 9. ADX / DI

ADX measures trend strength, not direction.

PLUTUS combines:
- ADX
- DI+
- DI-

Examples:
- rising ADX + DI+ dominance supports bullish continuation
- rising ADX + DI- dominance supports bearish continuation
- falling ADX after a mature expansion can support exhaustion/reversal evidence

ADX/DI qualify setups; they do not create signals by themselves.

---

## 10. Divergence

Divergence is used in two ways:
- capped global confluence modifier
- local institutional reaction evidence

Aligned divergence can strengthen an OB/Breaker reaction.

Divergence is not an equal fourth top-level PLUTUS pillar.

---

## 11. Signal Visual Settings

Recommended starting settings:

- Signal Label Detail: **Compact**
- Risk / Reward: ON
- Entry / Stop / Target Lines: ON
- Signal Price Labels: OFF
- Target R Multiple: 2.0
- Reaction Confirmation Window: 3
- Delayed Reaction Reclaim: Outside Zone
- Count Wick Touch: ON
- Require Directional Departure: ON

Use Detailed labels mainly while validating or studying signals.

---

## 12. Diagnostics

Diagnostic mode explains why a setup is waiting or blocked.

Common states:
- `WAIT_RECLAIM`
- `NO_SPACE`
- `CHOP`
- `HIGH_VOL`
- `COUNTERTREND_EVIDENCE`
- `REVERSAL_EVIDENCE`
- `OB_REACTION`
- `BREAKER_REACTION`
- `FLOW_ADX`
- `SQ`
- `CONFIRMED`

The diagnostic panel does not intentionally feed signal logic.

---

## 13. Release Validation Panel

Enable:

`Release Validation → Show Validation Panel`

It displays:
- total signals
- pending-state synchronization
- Trend / Countertrend / Reversal counts
- 1B / 2B / 3B / wick counts
- 1R and 2R reach rates
- average MFE and MAE
- ambiguous OHLC outcome bars
- last deterministic signal key

This panel is intended for testing and statistical review, not trade confirmation.

---

## 14. Known Limitations

- PLUTUS does not use native `request.footprint()`.
- Its volume profile is a PLUTUS approximation and is not claimed to exactly reproduce TradingView proprietary profile calculations.
- FX/CFD volume quality depends on the data feed.
- Zone inventory is bounded by the configured maximum; the current v1.0 release retains simple bounded-capacity behavior.
- Historical performance statistics from the validation panel are descriptive, not a guarantee of future results.

---

## 15. Suggested Workflow

1. Identify institutional zone.
2. Let price interact with the zone.
3. Wait for PLUTUS reaction confirmation.
4. Read regime:
   - TRD
   - CTR
   - REV
5. Check SQ and available reward space.
6. Use diagnostics if a visually interesting reaction does not signal.
7. Avoid changing thresholds based on one isolated trade.

---

## 16. Release Baseline

Frozen trading-model baseline:

`c2e8d0860164f98b372038f177a92c0bf9d0bfae`

Presentation/validation changes after the freeze do not intentionally modify signal-generation rules.
