# PLUTUS P09.1 — Signal Quality / Context Filtering Test Plan

## Objective
Validate that P09.1 preserves the P09.0 FVG-touch -> ARMED -> candle-close confirmation architecture while adding quality, regime, session, freshness, conflict and duplicate filtering.

## 1. Compile / runtime
- Pine Script v6 compiles without errors.
- No runtime array/object errors.
- Test on multiple intraday symbols and timeframes.

## 2. P09.0 state-machine regression
- Qualified FVG creates a SETUP only when the context gate passes.
- FVG touch changes SETUP -> ARMED.
- No signal is emitted merely because a wick touches the FVG.
- Confirmation occurs only on a confirmed candle close.
- Invalid close through the FVG boundary invalidates the setup.
- ARMED setup expires after Armed Setup Expiry.
- CONFIRMED advances into COOLDOWN/IDLE as designed.

## 3. Signal profiles
Test Aggressive, Balanced, Conservative and Custom.
Expected ordering:
- Aggressive produces the most permissive threshold set.
- Balanced is the baseline.
- Conservative requires materially stronger confluence/confidence/setup quality.
- Custom uses the user-defined thresholds.

## 4. Setup quality
Confirm quality responds to:
- FVG mechanical quality.
- Directional confluence.
- Confidence.
- Directional participation.
- Zone freshness.
Older FVGs should lose freshness contribution and should not become stronger solely because of age.

## 5. Participation confirmation
With Minimum Confirmation Participation > 0:
- Long confirmation requires positive participation at/above threshold.
- Short confirmation requires negative participation at/below inverse threshold.
- Weak/opposing participation blocks confirmation while ARMED.

## 6. Conflict rejection
With Reject Strong Opposing Evidence ON:
- Strong bearish participation/value blocks bullish context.
- Strong bullish participation/value blocks bearish context.
With it OFF, the same conflict must not independently block a setup.

## 7. Regime filter
- Off: regime does not block setups.
- Avoid Compression: low ATR-ratio or low ADX blocks setups.
- Directional Trend: long requires bullish DMI regime; short requires bearish DMI regime.

## 8. Session filter
Check:
- All
- London
- NY AM
- NY PM
- London + NY
London uses Europe/London and NY windows use America/New_York so DST follows the regional timezone.
Signals outside the selected session must be blocked.

## 9. VWAP-side gate
When enabled:
- Long close must be on/above daily VWAP.
- Short close must be on/below daily VWAP.
When disabled, VWAP side alone must not reject the setup.

## 10. Duplicate-zone suppression
With Allow Repeat Signal From Same FVG OFF:
- After one confirmed signal from an FVG, that same FVG must not generate another confirmed setup after cooldown.
- A newly created FVG can generate a new setup.
With it ON, repeat use is allowed after cooldown if the zone remains valid.

## 11. Risk / reward preview
On confirmation:
- Entry = confirmation candle close.
- Long stop = FVG lower invalidation boundary - ATR buffer.
- Short stop = FVG upper invalidation boundary + ATR buffer.
- Target respects selected R multiple.
- Risk/reward boxes are finite and bounded by Maximum Trade Previews.

## 12. Diagnostics
Diagnostics should expose:
- state/direction
- block/ready reason
- confluence and threshold
- confidence and threshold
- setup quality and threshold
- participation
- regime
- session permission
- zone score
- touch/close status
- entry/stop/target

## Acceptance gate
P09.1 is accepted when:
- It compiles/runs cleanly.
- P09.0 touch-to-close behavior is preserved.
- Aggressive/Balanced/Conservative ordering is sensible.
- session/regime/conflict/participation gates demonstrably filter setups.
- same-zone duplicate suppression works.
- confirmed risk/reward previews remain finite.
- no intrabar touch-only BUY/SELL is produced.

## Integration note
P09.1 intentionally uses compact local context proxies. P10 must replace them with the validated upstream P01-P07 outputs and must not treat this compact confluence as the final PLUTUS architecture.
