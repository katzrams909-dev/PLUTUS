# PLUTUS v1.0 — User Guide

## Overview

PLUTUS is an institutional participation and reaction indicator for TradingView.

Its purpose is to answer:

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

A zone touch does not automatically produce a signal. PLUTUS evaluates the quality of the reaction, the surrounding regime, participation, context, and available reward space before confirming.

---

## Signal Labels

The default label mode is now **Compact**.

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
- `CL REJ` — institutional-cluster rejection
- `CL SWP` — institutional-cluster sweep/reclaim

Use **Minimal** if you only want LONG/SHORT labels. Use **Detailed** when reviewing reaction timing or reward space.

---

## LONG / SHORT Colors

Under **Signal Visuals**:

- `LONG Signal Color`
- `SHORT Signal Color`

These control the direction-sensitive signal visuals:
- signal label
- entry line
- target line
- target/reward box
- entry and target price labels

The stop line and risk object remain neutral gray so LONG/SHORT color meaning stays visually clear.

---

## Reaction Timing

PLUTUS supports 1-, 2-, and 3-bar zone reactions.

### 1-bar reaction
Price enters or wicks into the zone and confirms immediately.

### 2-bar reaction
1. First bar retests the zone and may close inside it.
2. Second bar reclaims/departs in the intended direction.
3. Signal confirms on the second bar.

### 3-bar reaction
The first two bars may remain inside or absorb within the zone, provided the zone remains valid. The third bar can then provide the directional reclaim.

PLUTUS always uses the earliest valid confirmation inside the configured reaction window.

---

## Wick Interactions

A wick entering a zone counts as a valid interaction even if the candle body does not enter.

Example bullish reaction:
- wick trades into a Bull OB
- candle body remains above the block
- candle closes back above the zone
- the reaction may qualify

Mitigation settings and interaction detection are separate concepts.

---

## IFVG and Breaker Retests

IFVG and Breaker zones cannot signal on the candle that creates the flip.

Required sequence:

`source failure → flipped zone created → later retest → valid reaction → signal`

The flip candle itself is never counted as the retest.

---

## Trend / Countertrend / Reversal

Zone type does not determine the trade regime.

### Trend
The signal direction agrees with the prevailing directional regime.

### Countertrend
The setup opposes the prevailing regime but has sufficient evidence such as:
- strong location
- absorption
- divergence
- sweep/reclaim
- institutional clustering
- deteriorating directional strength

### Reversal
The existing regime is showing evidence of failure and the opposite direction is taking control.

Example:

`bull trend → Bear OB forms near high → selloff → retrace into Bear OB → bearish reaction → REV SHORT`

Later bearish reactions after the regime transitions may be classified as Trend shorts.

---

## Signal Quality — SQ

SQ measures the quality of the actual setup.

Its inputs include:
- source quality
- reaction quality
- participation / flow
- regime compatibility
- context / confidence
- reward space

A high SQ does not override a hard invalidation.

A setup can still be blocked for reasons such as:
- invalid zone
- insufficient reward space
- failed reaction
- strong opposing context
- session restriction
- chop / extreme-volatility veto

---

## Volume / Participation

PLUTUS evaluates relative participation rather than treating volume as a standalone trigger.

It considers:
- RVOL
- arrival pressure
- reaction pressure
- absorption
- directional departure

For FX and CFD markets, volume may be broker/feed-dependent tick volume. It should therefore be interpreted as relative participation rather than centralized exchange volume.

---

## ADX / DI

ADX measures trend strength, not direction.

PLUTUS uses:
- ADX
- DI+
- DI-

Examples:
- rising ADX + DI+ dominance supports bullish continuation
- rising ADX + DI- dominance supports bearish continuation
- falling ADX after mature expansion may support exhaustion / reversal evidence

ADX/DI qualify setups; they do not generate trades by themselves.

---

## Divergence

Divergence is used as:
- a capped global confluence modifier
- local evidence for institutional reactions such as OB/Breaker retests

Aligned divergence can strengthen a setup. Opposing divergence can reduce quality or contribute to a veto.

---

## Diagnostics

Diagnostic mode helps explain why a visually interesting setup did not signal.

Common states include:
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

Diagnostics are intended for explanation and troubleshooting, not as additional trade triggers.

---

## Recommended Starting Settings

- Signal Label Detail: **Compact**
- Show Risk / Reward: ON
- Show Entry / Stop / Target Lines: ON
- Show Signal Price Labels: OFF
- Target R Multiple: 2.0
- Reaction Confirmation Window: 3
- Delayed Reaction Reclaim: Outside Zone
- Count Wick Touch / Close-Out Rejection: ON
- Require Directional Departure: ON
- Effort / Result Filter: Boost

---

## Known Limitations

- PLUTUS does not use native `request.footprint()`.
- Its profile calculations are PLUTUS approximations and do not claim exact TradingView proprietary profile equivalence.
- FX/CFD volume quality depends on the data feed.
- Zone inventory is bounded by the configured maximum.
- Signals are analytical outputs, not guarantees of future performance.

---

## Release Baseline

Frozen trading-model baseline:

`c2e8d0860164f98b372038f177a92c0bf9d0bfae`

The label and color changes described in this guide are presentation-only and do not intentionally alter the frozen trading model.
