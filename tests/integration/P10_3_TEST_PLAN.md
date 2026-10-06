# PLUTUS P10.3 — Release Hardening Test Plan

## Objective
Validate the hardened integrated build before release-candidate packaging. P10.3 adds no new trading model; it closes known implementation debts and adds auditability.

## Compile / Runtime
- [ ] Pine Script v6 compiles cleanly.
- [ ] No runtime array-bounds errors.
- [ ] No object-limit errors over long history.
- [ ] No request.footprint() usage.
- [ ] Display toggles do not alter calculations.

## HLC3 Profile Allocation
- [ ] HLC3 Point mode uses actual stored candle close.
- [ ] HLC3 formula is exactly (high + low + close) / 3.
- [ ] Profile close array resets with the selected profile scope.
- [ ] Profile close array shifts with high/low/volume when max bars is reached.
- [ ] Range Overlap remains the default and is unchanged.
- [ ] POC/VAH/VAL remain internal values only.

## Divergence Parameter Guard
- [ ] Effective maximum pivot separation is never below minimum separation.
- [ ] Setting user Maximum Pivot Separation below Minimum Pivot Separation does not create impossible divergence-window behavior.
- [ ] Existing regular/hidden divergence behavior remains otherwise unchanged.

## Cluster Pair Re-entry Identity
- [ ] Same Breaker + FVG/IFVG pair is treated as the same cluster regardless of which member is scanned first.
- [ ] Same cluster pair cannot bypass re-entry delay by swapping primary/secondary member order.
- [ ] A different cluster sharing only one member is not incorrectly blocked as the same pair.
- [ ] Single-zone re-entry behavior remains unchanged.

## Signal Risk Guard
- [ ] A setup with risk below one minimum tick cannot confirm.
- [ ] No zero-width risk/reward box is created.
- [ ] No entry=stop trade preview is created.
- [ ] Normal valid-risk setups are unchanged.
- [ ] Stop remains zone boundary plus ATR buffer.
- [ ] Target remains configured R multiple.

## Signal Audit
- [ ] Diagnostic Signal Audit table appears only when diagnostics are enabled.
- [ ] Current signal state is shown.
- [ ] Setup interaction class is shown.
- [ ] Stable zone/cluster provenance is shown.
- [ ] Direction/type is shown.
- [ ] Direction/type/cluster invariant status is shown.
- [ ] Effort/result and quality are shown.
- [ ] P10 confluence/confidence are shown.
- [ ] Risk guard PASS/BLOCK is shown.
- [ ] Last confirmed signal provenance persists for review.
- [ ] Audit table is display-only and cannot affect signal logic.

## Zone / Signal Regression
- [ ] Bull FVG rejection/sweep cannot produce SHORT.
- [ ] Bear FVG rejection/sweep cannot produce LONG.
- [ ] IFVG/Breaker follows live flipped-zone direction.
- [ ] ZONE_FAILURE remains the only opposite-source-direction class.
- [ ] Multi-bar drift does not create IFVG/Breaker.
- [ ] Prior-close -> current-close single-bar inversion still works.
- [ ] Terminal-fill FVG/IFVG/Breaker remains eligible on fill bar then retires.
- [ ] Retired zones cannot signal later.
- [ ] Stable IDs remain correct after array shifts/removals.

## Cluster Regression
- [ ] Same-direction Breaker + FVG cluster still forms.
- [ ] Same-direction Breaker + IFVG cluster still forms.
- [ ] Opposite-direction zones never form a cluster.
- [ ] Intersection bounds remain the operative cluster range.
- [ ] Cluster member retirement invalidates the armed cluster.
- [ ] CLUSTER_REJECTION remains directional.
- [ ] CLUSTER_SWEEP_RECLAIM remains directional.

## Effort vs Result Regression
- [ ] Off mode does not gate signals.
- [ ] Boost mode affects quality only.
- [ ] Require mode enforces Minimum Absorption Score.
- [ ] Effort/result does not become a fourth P10 pillar.

## P10 Core Regression
- [ ] Participation pillar unchanged.
- [ ] Value/Auction pillar still internally blends P02/P03/P04.
- [ ] Institutional pillar unchanged.
- [ ] P06 remains capped modifier.
- [ ] Confidence still uses availability/reliability/agreement/conflict.
- [ ] Session boxes remain unchanged.
- [ ] VWAP reset breaks remain unchanged.
- [ ] Histogram behavior remains unchanged.
- [ ] Status panel remains readable.

## Historical Stress Review
Run at minimum on:
- [ ] XAUUSD 15m
- [ ] NAS100 5m / 15m
- [ ] SPX500 5m / 15m
- [ ] EURUSD 15m
- [ ] GBPUSD 15m
- [ ] USDJPY 15m
- [ ] USDCAD 15m

Review:
- [ ] No visually stale filled flipped zones.
- [ ] No wrong-direction entries.
- [ ] No duplicate cluster entries inside re-entry delay.
- [ ] No obviously delayed inversion signal caused by visual-object deletion.
- [ ] No runaway lines/boxes.
- [ ] No unexplained signal without provenance.

## Release Candidate Gate
P10.3 is accepted when:
1. It compiles cleanly.
2. The historical stress set shows no critical lifecycle or direction regressions.
3. Signal provenance explains every reviewed entry.
4. No runtime/object-limit issue is observed.
5. No new trading-model changes are required.

After acceptance, package the release candidate from P10.3.
