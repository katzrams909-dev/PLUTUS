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
