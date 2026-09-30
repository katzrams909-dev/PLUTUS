# PLUTUS P09.0 — Signal Architecture Test Plan

## Purpose
Validate the first PLUTUS signal state machine using the agreed entry model:

1. Qualified FVG exists.
2. PLUTUS context permits the direction.
3. Price touches / enters the FVG.
4. Touch arms the setup.
5. Candle close validates or rejects the setup.
6. Confirmed event records entry / invalidation / target data.

This prototype is for state-machine validation. Final P10 integration must replace compact local context proxies with the exact validated P01–P07 outputs.

## Compile / runtime gate
- Pine Script v6 compiles without errors.
- No runtime array/object errors.
- No signal object extends indefinitely.

## FVG trigger checks
- Bullish FVG uses `low > high[2]`.
- Bearish FVG uses `high < low[2]`.
- Minimum tick and ATR width filters work.
- Displacement, candle efficiency and RVOL qualification work.
- FVG is not eligible for its own touch on the birth bar.
- Active FVG box stops when invalidated or expired.

## Signal-state checks
Expected states:
- `IDLE`
- `SETUP`
- `ARMED`
- `CONFIRMED`
- `INVALIDATED`
- `EXPIRED`
- `COOLDOWN`

Validate:
- A qualified directional FVG with valid context creates `SETUP`.
- First later touch changes setup to `ARMED`.
- Intrabar touch alone does not create a confirmed signal.
- Confirmation is evaluated only on a confirmed candle close.
- Touch-bar confirmation can be enabled/disabled.
- Broken zone invalidates the setup.
- Armed setup expires after configured bars.
- Confirmed signals enter cooldown.
- No new setup is accepted during cooldown.

## Close confirmation modes
Test each mode:
- `Directional Close`
- `Close Back Through Midpoint`
- `Any Valid Close`

For Directional Close, confirm the candle body direction and close-location threshold are respected.

## Context gate checks
- Minimum directional confluence filters setups.
- Minimum confidence filters setups.
- Optional correct-side-of-Daily-VWAP filter works.
- Bearish conditions mirror bullish conditions.

## Risk / reward preview
On confirmation:
- Entry equals confirmation candle close.
- Long stop is below FVG invalidation boundary plus ATR buffer.
- Short stop is above FVG invalidation boundary plus ATR buffer.
- Target respects configured R multiple.
- Gray risk box and green reward box are finite-width objects.
- Old previews are removed when the configured object limit is exceeded.

## Visual / object discipline
- FVG trigger boxes use `extend.none`.
- Trade preview boxes use finite `bar_index + Preview Width` right edges.
- Signal labels are bounded by `Maximum Trade Previews`.
- Current-state label is replaced rather than accumulated.

## Diagnostics
When diagnostics are enabled verify:
- state
- direction
- trigger type
- confluence
- confidence
- zone score
- entry
- stop
- target
- touch / close validity

## Alert checks
- Long alert fires only on confirmed bullish event.
- Short alert fires only on confirmed bearish event.
- No alert fires from FVG touch alone.

## Acceptance gate
P09.0 is accepted when:
- it compiles and runs cleanly,
- FVG touch arms but does not independently confirm,
- confirmation occurs at candle close when filters pass,
- invalidation / expiry / cooldown behave correctly,
- risk/reward previews are finite,
- no repaint-style intrabar confirmation is used for historical confirmed signals.
