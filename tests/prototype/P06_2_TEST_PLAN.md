# PLUTUS P06.2 — Unified Divergence Test Plan

## Objective
Validate that P06.2 correctly consolidates confirmed regular/hidden divergence events into independent bullish and bearish inventories and produces stable downstream-ready divergence state outputs.

## Compile gate
- Pine Script v6 compiles without errors.
- No runtime array/object errors.

## Core divergence regression
Test both RSI and MACD Histogram modes.

Verify:
- regular bullish = price LL + oscillator HL
- hidden bullish = price HL + oscillator LL
- regular bearish = price HH + oscillator LH
- hidden bearish = price LH + oscillator HH
- pivots are confirmed only after `rightBars`
- no future-bar/repainting dependency is introduced

## Quality regression
Confirm P06.1 filters remain effective:
- minimum/maximum pivot separation
- minimum ATR price separation
- pivot prominence
- oscillator separation
- quality threshold
- duplicate cooldown
- participation confirmation toggle
- daily VWAP context toggle

## Independent directional inventories
Create or locate sequences where bullish and bearish divergence events occur within the same expiry window.

Verify:
- bullish event remains active when a later bearish event appears
- bearish event remains active when a later bullish event appears
- each direction has independent age, type, score and quality
- each direction expires independently after `eventExpiryBars`

## Type tracking
Verify strongest/current active directional event reports:
- `REGULAR`
- `HIDDEN`
- `NONE` after expiry

## Unified score
Expected model:
- bullish inventory is positive 0–100
- bearish inventory is positive 0–100
- unified score = bullish inventory - bearish inventory
- final score remains within -100..100

Check:
- only bullish active => positive score
- only bearish active => negative score
- both active with unequal strength => net direction follows stronger inventory
- no events => 0

## Conflict handling
With both bullish and bearish inventories active:
- if absolute score difference <= `conflictDifference`, state must be `DIVERGENCE_CONFLICT`
- if difference exceeds threshold, stronger side must control directional state
- conflict must not create a trade signal

## State model
Verify categorical outputs:
- `DIVERGENCE_NONE`
- `DIVERGENCE_BULLISH`
- `DIVERGENCE_STRONGLY_BULLISH`
- `DIVERGENCE_BEARISH`
- `DIVERGENCE_STRONGLY_BEARISH`
- `DIVERGENCE_CONFLICT`
- `FORMING` on unconfirmed realtime bar

## Confidence
Verify:
- one active side => confidence tracks that event quality
- opposing events reduce confidence as directional dominance falls
- near-equal opposing events produce low confidence
- no active events => 0 confidence

## Lifecycle
Check event ages and expiry:
- age increments from confirmed pivot bar
- active state persists only through configured expiry
- expired bullish event does not erase a still-valid bearish event, and vice versa

## Object discipline
- divergence lines/labels remain bounded by `maxEvents`
- regular divergence uses solid lines
- hidden divergence uses dashed lines
- turning lines or labels off must not affect calculation/state logic

## Diagnostics
Verify table values agree with visible conditions:
- bull/bear active
- bull/bear type
- bull/bear score
- bull/bear quality
- bull/bear age
- conflict
- unified score
- confidence
- state

## Acceptance gate
P06.2 is accepted when:
1. it compiles and runs without errors,
2. confirmed divergence mechanics match P06.0/P06.1,
3. bullish and bearish inventories coexist and expire independently,
4. conflict handling behaves logically,
5. unified score/confidence/state are stable and non-repainting,
6. visuals remain bounded and optional.
