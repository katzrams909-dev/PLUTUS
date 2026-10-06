# PLUTUS P09.2 — Interaction-First Multi-Zone Entry Test Plan

## Objective
Validate the revised P09.2 architecture where institutional-zone interaction comes first and confirmation-time context decides whether the event becomes a signal.

Supported interaction classes:
- REJECTION
- SWEEP_RECLAIM
- BREAK_RECLAIM
- FLIP_RETEST

Supported zone families:
- FVG
- IFVG
- Order Block
- Breaker Block

## Compile / Runtime
- [ ] Pine Script v6 compiles without errors.
- [ ] No runtime array bounds errors.
- [ ] Zone inventory stays within `Maximum Tracked Zones`.
- [ ] Signal/risk/reward objects stay bounded by configured preview limits.

## Interaction-First Architecture
- [ ] A qualified zone can arm from price interaction even if directional confluence is temporarily unfavorable.
- [ ] Confluence is not required before the touch/reclaim event.
- [ ] Confirmation-time context is required before a BUY/SELL signal.
- [ ] The exact source zone remains bound by stable zone ID through confirmation/invalidation.
- [ ] A newer zone cannot silently replace the armed source zone.

## Interaction Classification
### REJECTION
- [ ] Normal touch of a fresh FVG/OB arms a rejection event.
- [ ] Confirmation requires a valid directional close plus confirmation-time gates.

### SWEEP_RECLAIM
- [ ] Wick can penetrate through the far side of a zone without automatically invalidating the setup.
- [ ] Sweep must remain within `Maximum Sweep Beyond Zone / ATR`.
- [ ] Price must close back into/through the valid side of the zone.
- [ ] Decisive close-through remains invalidation rather than a sweep.

### BREAK_RECLAIM
- [ ] Decisive close through a bearish source zone can arm a LONG reclaim.
- [ ] Decisive close through a bullish source zone can arm a SHORT reclaim.
- [ ] Trade direction is allowed to differ from the source zone direction.
- [ ] Confirmation requires price to remain beyond the reclaimed side of the zone.
- [ ] The setup remains valid if the same source zone converts to IFVG/Breaker.
- [ ] Failed inversion/removal invalidates the reclaim setup.

### FLIP_RETEST
- [ ] Retest of an IFVG arms in the IFVG direction.
- [ ] Retest of a Breaker arms in the Breaker direction.
- [ ] Flip retest uses the same confirmed-close discipline as other entries.

## Institutional Cluster Quality
- [ ] Overlapping same-direction fresh zones are counted as one supporting cluster.
- [ ] Cluster membership requires configured overlap percentage.
- [ ] Additional overlapping zones increase cluster quality by the configured boost.
- [ ] The signal still binds to one exact source zone ID.
- [ ] Cluster scoring improves quality without creating duplicate signals from every overlapping zone.

## Confirmation-Time Gates
- [ ] Directional confluence threshold is evaluated at confirmation.
- [ ] Confidence threshold is evaluated at confirmation.
- [ ] Participation threshold is evaluated at confirmation.
- [ ] VWAP-side gate works when enabled.
- [ ] Strong opposing evidence can reject confirmation.
- [ ] Regime filter is evaluated at confirmation.
- [ ] Session filter is evaluated at confirmation.
- [ ] Setup quality is recalculated using cluster quality, confluence, confidence, participation and freshness.

## State Machine
Validate:
`IDLE -> ARMED -> CONFIRMED / INVALIDATED / EXPIRED -> COOLDOWN -> IDLE`

- [ ] Interaction arms the setup.
- [ ] No signal is emitted solely from touch.
- [ ] Same-bar close confirmation follows the configured toggle.
- [ ] Otherwise the engine can wait through the configured armed window.
- [ ] Context can improve after touch and still confirm before expiry.
- [ ] Bound-zone disappearance invalidates the setup.
- [ ] Armed setup expires after configured bars.
- [ ] Cooldown blocks immediate repeat signals.
- [ ] Repeat-same-zone toggle behaves correctly.

## Risk / Reward
- [ ] Entry equals confirmation candle close.
- [ ] Long stop is below the bound zone plus ATR buffer.
- [ ] Short stop is above the bound zone plus ATR buffer.
- [ ] Reclaim trades use the same zone boundaries for invalidation.
- [ ] Target is derived from configured R multiple.
- [ ] Risk and reward boxes remain finite.

## Diagnostics
- [ ] Diagnostics show interaction type.
- [ ] Diagnostics show trigger type and stable zone ID.
- [ ] Diagnostics show cluster count and cluster quality.
- [ ] Diagnostics show touch depth.
- [ ] Diagnostics show confluence, confidence, participation, regime and session state.
- [ ] Diagnostics clearly show WAIT CONTEXT / WAIT PARTICIPATION / WAIT CLOSE when armed.
- [ ] Diagnostics show entry, stop and target after confirmation.

## Screenshot Regression Cases
Use the supplied yellow-circle examples as regression references:
- [ ] NAS100 15m overlapping Bull FVG + Bull OB reactions.
- [ ] EURUSD 15m bearish-zone reclaim LONG case.
- [ ] EURUSD 15m bearish-zone retest SHORT case.
- [ ] USDJPY 15m deep Bull FVG sweep/reclaim.
- [ ] NAS100 5m sharp Bull FVG touch/recovery.
- [ ] US30 5m Bear FVG continuation rejection.
- [ ] EURUSD 5m bearish-zone reclaim LONG and later bearish retest SHORT.

## Acceptance Gate
P09.2 revision passes when:
1. It compiles/runs without array errors.
2. Interaction can arm before confluence turns favorable.
3. REJECTION, SWEEP_RECLAIM, BREAK_RECLAIM and FLIP_RETEST can all reach confirmation.
4. Confirmation still requires valid close + quality/context.
5. Zone direction and trade direction can differ for reclaim setups.
6. Overlapping institutional zones strengthen quality without producing uncontrolled duplicate entries.
7. Risk/reward and zone visuals remain finite and bounded.

## Integration Note
P09.2 still uses compact local participation/value/confluence evidence. P10 must bind this state machine to the exact validated P01-P07 outputs and P05 zone inventory.
