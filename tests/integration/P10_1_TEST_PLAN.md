# PLUTUS P10.1 — Integrated Signals Test Plan

## Objective
Validate the first full production merge of the accepted P08 visual core, P10 three-pillar confluence, strict institutional-zone lifecycle, and P09 interaction-first signal engine.

## Compile / Runtime
- [ ] Pine Script v6 compiles cleanly.
- [ ] No runtime array bounds errors.
- [ ] No object-limit errors during long historical runs.
- [ ] No request.footprint() usage.

## Zone Identity / Provenance
- [ ] Every live zone has a stable numeric ID.
- [ ] Signal labels include zone direction, type, and ID.
- [ ] Armed setup remains bound to the exact zone ID through confirmation/invalidation.
- [ ] A newer overlapping zone cannot silently replace the armed source.

## Strict Inversion
- [ ] Bull FVG/OB flips only when prior close is at/above source top and current close closes below far boundary.
- [ ] Bear FVG/OB flips only when prior close is at/below source bottom and current close closes above far boundary.
- [ ] Multi-bar drift through the source does not manufacture an IFVG/Breaker.
- [ ] Valid FVG failure becomes IFVG immediately.
- [ ] Valid OB failure becomes Breaker immediately.
- [ ] JUST_FLIPPED is available on the inversion bar for immediate zone-failure evaluation.

## Fill / Retirement
- [ ] FVG full fill produces TERMINAL_FILL and remains eligible on that bar.
- [ ] IFVG/Breaker full fill produces TERMINAL_FILL and remains eligible on that bar.
- [ ] Terminal zones retire on the next bar.
- [ ] Retired filled zones cannot continue generating signals.
- [ ] OBs are not automatically retired solely by partial mitigation.

## Signal Direction Invariants
- [ ] Bull FVG rejection/sweep -> LONG only.
- [ ] Bear FVG rejection/sweep -> SHORT only.
- [ ] Bull OB rejection/sweep -> LONG only.
- [ ] Bear OB rejection/sweep -> SHORT only.
- [ ] Bull IFVG/Breaker retest -> LONG only.
- [ ] Bear IFVG/Breaker retest -> SHORT only.
- [ ] ZONE_FAILURE is the only class allowed to trade opposite the original source direction.

## P10 Context Integration
- [ ] Signal confirmation uses P10 confluenceScore.
- [ ] Signal confirmation uses P10 confidence.
- [ ] No compact P09 confluence proxy is present.
- [ ] P01 participation is used as confirmation participation evidence.
- [ ] P06 remains only a capped modifier through P10 confluence.

## Interaction Classes
- [ ] REJECTION confirms on a valid directional/selected close.
- [ ] SWEEP_RECLAIM tolerates configured wick penetration and confirms in zone direction.
- [ ] ZONE_FAILURE can confirm on the inversion bar when speed/context conditions pass.
- [ ] FLIP_RETEST confirms only in the live IFVG/Breaker direction.

## Signal Lifecycle
- [ ] IDLE -> ARMED -> CONFIRMED / INVALIDATED / EXPIRED -> IDLE.
- [ ] Touch/interaction arms first.
- [ ] No trade signal is emitted from touch alone.
- [ ] Same-bar close confirmation obeys its toggle.
- [ ] Armed setup expires after configured bars.
- [ ] Directional cooldowns suppress rapid duplicates.
- [ ] Same-zone re-entry delay works independently of different zones.

## Risk / Reward / Visuals
- [ ] Entry equals confirmation close.
- [ ] Stop uses the bound zone boundary plus ATR buffer.
- [ ] Target uses configured R multiple.
- [ ] Risk/reward boxes are finite.
- [ ] Entry/stop/target lines are finite.
- [ ] Signal labels are bounded by Maximum Signal Previews.
- [ ] Minimal / Compact / Detailed labels do not change logic.

## Alerts
- [ ] LONG.
- [ ] SHORT.
- [ ] REJECTION.
- [ ] SWEEP_RECLAIM.
- [ ] ZONE_FAILURE.
- [ ] FLIP_RETEST.
- [ ] Alerts fire only on confirmed-bar signals.

## P08 Visual Regression
- [ ] VWAP remains reset-safe.
- [ ] Unified profile remains unchanged.
- [ ] Session boxes remain unchanged.
- [ ] Status panel remains readable.
- [ ] POC/VAH/VAL remain internal only.
- [ ] Hiding visuals does not affect signal logic.

## Acceptance Gate
P10.1 passes when it compiles cleanly and the previously validated P09 cases behave correctly while using P10 three-pillar context.
