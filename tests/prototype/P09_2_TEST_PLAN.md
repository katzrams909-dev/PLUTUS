# PLUTUS P09.2 — Multi-Zone Entry Test Plan

## Objective
Validate extension of the P09.1 signal state machine from FVG-only triggers to the institutional-zone family: FVG, IFVG, Order Block and Breaker Block.

## Compile / Runtime
- [ ] Pine Script v6 compiles without errors.
- [ ] No runtime array bounds errors.
- [ ] Zone inventory stays within `Maximum Tracked Zones`.
- [ ] Signal/risk/reward objects stay bounded by configured preview limits.

## Zone Detection
- [ ] Bullish and bearish FVG candidates appear only after displacement/width/quality gates.
- [ ] Bullish and bearish OB candidates require displacement and local structure consequence.
- [ ] Duplicate/overlapping zones are suppressed according to configured thresholds.
- [ ] Zone boxes do not extend indefinitely; active boxes end at the current bar and are deleted on lifecycle termination.

## IFVG / Breaker Conversion
- [ ] Invalidated FVG can arm an inversion only after decisive close through the zone.
- [ ] Follow-through within the confirmation window converts FVG -> IFVG.
- [ ] Invalidated OB can arm a breaker only after decisive close through the zone.
- [ ] Follow-through within the confirmation window converts OB -> BREAKER.
- [ ] Failed inversion confirmation removes the invalidated source zone.
- [ ] Inversion score factor and minimum inversion score are respected.

## Trigger Selection
- [ ] FVG / IFVG / OB / Breaker entry toggles work independently.
- [ ] Best eligible bullish and bearish zones are selected by live setup quality.
- [ ] Signal setup binds to a stable zone ID, not merely the latest same-direction zone.
- [ ] New zones appearing later do not silently replace an already-bound setup.

## State Machine
Validate:
`IDLE -> SETUP -> ARMED -> CONFIRMED / INVALIDATED / EXPIRED -> COOLDOWN -> IDLE`

- [ ] Touch of the bound zone arms the setup only after zone birth.
- [ ] No intrabar signal is emitted merely from touch.
- [ ] Candle-close confirmation is required.
- [ ] Same-touch-bar confirmation follows the configured toggle.
- [ ] Setup invalidates if its exact bound zone disappears/converts/changes direction.
- [ ] Armed setup expires after configured bars.
- [ ] Cooldown blocks immediate repeat signals.
- [ ] Repeat-same-zone toggle behaves correctly.

## Signal Quality
- [ ] Aggressive / Balanced / Conservative / Custom thresholds behave as expected.
- [ ] Confluence threshold is directional.
- [ ] Confidence threshold is enforced.
- [ ] Setup quality includes zone quality, confluence, confidence, participation and freshness.
- [ ] Strong opposing evidence can reject setups.
- [ ] Participation is checked again on confirmation.
- [ ] VWAP-side gate works when enabled.

## Regime / Session
- [ ] Avoid Compression blocks low-ATR/low-ADX conditions.
- [ ] Directional Trend requires matching DMI direction.
- [ ] London / NY AM / NY PM / London + NY filters work with regional DST-aware timezones.

## Risk / Reward
- [ ] Entry equals confirmation candle close.
- [ ] Long stop is below the bound zone plus ATR buffer.
- [ ] Short stop is above the bound zone plus ATR buffer.
- [ ] Target is derived from configured R multiple.
- [ ] Risk and reward boxes are finite and do not extend forever.
- [ ] Signal label includes trigger type and setup quality.

## Diagnostics
- [ ] Diagnostics display state, direction, trigger type and stable zone ID.
- [ ] Diagnostics show confluence, confidence, setup quality, participation, regime and session state.
- [ ] Diagnostics show entry, stop and target after confirmation.

## Acceptance Gate
P09.2 passes when:
1. It compiles/runs without array errors.
2. All four trigger families can feed the same state machine.
3. Setup remains bound to the exact triggering zone.
4. IFVG/Breaker conversions behave directionally and finitely.
5. Touch only arms; confirmed-bar validation creates the signal.
6. Risk/reward and zone visuals remain finite and bounded.

## Integration Note
P09.2 still uses compact local participation/value/confluence evidence. P10 must bind this state machine to the exact validated upstream PLUTUS engines and P05 zone inventory instead of duplicating proxy logic.
