# PLUTUS P09.3 — Signal Presentation Test Plan

## Objective
Validate final signal visuals, alert granularity and finite trade presentation while preserving the P09.2 interaction-first entry logic.

## Compile / Runtime
- [ ] Pine Script v6 compiles without errors.
- [ ] No runtime object-limit or array errors.
- [ ] P09.2 signal timing/logic remains unchanged by display settings.

## Signal Labels
- [ ] Minimal mode shows only LONG / SHORT.
- [ ] Compact mode shows side + interaction class.
- [ ] Detailed mode shows PLUTUS side + interaction + trigger type + quality.
- [ ] Label detail changes do not alter signal logic.
- [ ] Historical signal labels remain bounded by Maximum Trade Previews.

## Armed Marker
- [ ] Optional armed marker appears only while a setup is ARMED.
- [ ] Armed marker is deleted/recreated only for presentation on the last bar.
- [ ] Hiding the armed marker does not affect confirmation logic.

## Entry / Stop / Target Visuals
- [ ] Entry line begins on the confirmation bar and ends after configured Signal Level Width.
- [ ] Stop line is finite and at the calculated invalidation level.
- [ ] Target line is finite and at the calculated R target.
- [ ] Entry/stop/target line toggles operate independently.
- [ ] Price labels show exact TradingView mintick-formatted levels.
- [ ] Price labels remain bounded with signal history.
- [ ] Risk/reward boxes remain finite and unchanged from P09.2 behavior.

## Alerts
Validate separate alertconditions for:
- [ ] Generic LONG confirmation.
- [ ] Generic SHORT confirmation.
- [ ] REJECTION.
- [ ] SWEEP_RECLAIM.
- [ ] ZONE_FAILURE.
- [ ] BREAK_RECLAIM.
- [ ] FLIP_RETEST.

- [ ] Interaction-specific alerts fire only on confirmed signals.
- [ ] No alert fires merely because a setup becomes ARMED.
- [ ] Alerts remain confirmed-bar based.

## Regression
- [ ] FVG rejection timing matches P09.2.
- [ ] Sweep/reclaim timing matches P09.2.
- [ ] Same-zone re-entry behavior matches P09.2.
- [ ] Directional cooldown behavior matches P09.2.
- [ ] Stronger-interaction re-arming behavior matches P09.2.
- [ ] Zone-failure confirmation-speed behavior matches P09.2.
- [ ] FVG / IFVG / OB / Breaker triggers remain available.

## Known Integration Requirement
P09.3 inherits the current P09.2 source-zone lifecycle limitation: if an OB/FVG object is removed before an immediate failed-zone entry is fully evaluated, the signal may be late. P10 should preserve lightweight invalidated-source-zone metadata independently of whether the visual object still exists.

## Acceptance Gate
P09.3 passes when:
1. It compiles cleanly.
2. Display toggles never alter signal generation.
3. Signal levels and risk/reward previews remain finite.
4. Signal labels are readable in all three detail modes.
5. All seven alert families behave correctly.
6. P09.2 timing/behavior is preserved.


## Zone Lifecycle Revision
### FVG / IFVG / Breaker
- [ ] Fresh FVG remains live while unfilled/partially filled.
- [ ] Full fill marks FVG as `TERMINAL_FILL` for the fill bar.
- [ ] The terminal fill bar can still generate a valid directional signal.
- [ ] Filled FVG retires on the following bar when retirement is enabled.
- [ ] IFVG and Breaker behave the same way on full fill: one final signal opportunity, then retire.
- [ ] A retired filled flipped zone cannot generate later signals.

### Strict Single-Candle Inversion
- [ ] Bullish source FVG/OB flips only when one candle opens at/above the source top and closes below the far boundary.
- [ ] Bearish source FVG/OB flips only when one candle opens at/below the source bottom and closes above the far boundary.
- [ ] Multi-bar drift through a source zone does NOT create an IFVG or Breaker.
- [ ] Valid single-candle FVG failure converts immediately to IFVG when inversion score passes.
- [ ] Valid single-candle OB failure converts immediately to Breaker when inversion score passes.
- [ ] `JUST_FLIPPED` is exposed for the inversion bar so P09 can evaluate an immediate ZONE_FAILURE entry.
- [ ] The flipped zone transitions to normal `FRESH` state on the next bar.

### Order Block Mitigation
- [ ] OB revisit count increments only when price transitions from outside the zone into the zone.
- [ ] Consecutive candles remaining inside the same OB do not count as repeated revisits.
- [ ] OB score decays by the configured percentage on each distinct revisit.
- [ ] OB becomes `WEAK` when its live score drops below the configured minimum.
- [ ] Weak OB remains visible/trackable but carries its reduced score into signal quality.
- [ ] OB inversion still requires a full one-candle traverse; repeated mitigation alone does not create a Breaker.

### Visual / Logical Separation
- [ ] Visual disappearance occurs only after the terminal fill bar has been available to P09.
- [ ] Hiding zone visuals does not alter fill, retirement, touch decay or signal logic.
- [ ] Diagnostics show current lifecycle state and OB revisit count.

## Regression Requirement
- [ ] Previously stale IFVG/Breaker zones that kept producing signals after complete fill now retire.
- [ ] A full-fill rejection can still generate one final signal in the zone direction before retirement.
- [ ] Multi-candle closes progressively through an OB/FVG no longer manufacture a flipped zone.


## Inversion Traversal Correction
- [ ] Bullish FVG/OB can invert bearish when the prior close is at/above the source top and the current candle closes below the far boundary.
- [ ] Bearish FVG/OB can invert bullish when the prior close is at/below the source bottom and the current candle closes above the far boundary.
- [ ] Current candle open may be slightly inside the source zone and still qualify.
- [ ] Multi-bar drift still does not qualify because the immediately prior close must remain on the original side.
- [ ] XAUUSD 15m regression case forms a bearish IFVG and becomes eligible for a SELL.
