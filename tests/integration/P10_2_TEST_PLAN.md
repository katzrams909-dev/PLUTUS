# PLUTUS P10.2 — Cluster + Effort/Result Test Plan

## Objective
Validate institutional overlap clusters and Wyckoff-style effort/result reaction quality on top of the accepted P10.1 integrated signal build.

## Compile / Runtime
- [ ] Pine Script v6 compiles cleanly.
- [ ] No runtime array bounds errors.
- [ ] No object-limit errors on long histories.
- [ ] No request.footprint() usage.

## Institutional Cluster / "Unicorn" Logic
- [ ] Cluster requires same-direction zones.
- [ ] Supported pairs: Breaker + FVG and Breaker + IFVG.
- [ ] Opposite-direction overlap never creates a cluster.
- [ ] Cluster requires configured minimum overlap.
- [ ] Cluster bounds equal the actual price intersection of both member zones.
- [ ] When configured, price must touch the intersection itself, not merely one member zone.
- [ ] Cluster quality uses both member scores plus the configured cluster boost.
- [ ] Cluster provenance includes both stable zone IDs and types.
- [ ] Bull cluster can only generate LONG.
- [ ] Bear cluster can only generate SHORT.
- [ ] If either cluster member disappears or direction/type relationship breaks while armed, the setup invalidates.
- [ ] Cluster signal classes are CLUSTER_REJECTION and CLUSTER_SWEEP_RECLAIM.
- [ ] Cluster alerts fire only on confirmed signals.

## Cluster Lifecycle
- [ ] A terminal-fill member remains usable only for its terminal bar.
- [ ] Once a required cluster member retires, the cluster setup cannot confirm later.
- [ ] Cluster bounds update to the live intersection while both members remain valid.
- [ ] Cluster logic does not create new persistent chart objects that outlive the member zones.

## Effort vs Result / Absorption
- [ ] Effort uses relative volume.
- [ ] Result uses body efficiency / failed progress.
- [ ] Bull absorption incorporates lower-wick rejection and close recovery.
- [ ] Bear absorption incorporates upper-wick rejection and close deterioration.
- [ ] Off mode does not gate signals.
- [ ] Boost mode modifies interaction quality but does not hard-block low-absorption setups.
- [ ] Require mode blocks signals below Minimum Absorption Score.
- [ ] Absorption is used as reaction evidence only and does not become a fourth P10 pillar.
- [ ] Detailed labels expose ER score.

## Regression
- [ ] P10 three-pillar confluence remains unchanged.
- [ ] Existing FVG, IFVG, OB and Breaker signals still follow hard direction invariants.
- [ ] Strict prior-close/current-close inversion remains unchanged.
- [ ] Filled FVG/IFVG/Breaker retirement remains unchanged.
- [ ] Session/VWAP/profile behavior remains unchanged.
- [ ] Hiding visuals does not change cluster or effort/result logic.

## Chart Cases To Review
- [ ] Breaker + FVG overlap followed by directional rejection.
- [ ] Breaker + IFVG overlap followed by directional rejection.
- [ ] Sweep through cluster intersection followed by reclaim.
- [ ] High-RVOL, low-result absorption reaction.
- [ ] Similar-looking rejection with weak volume should score lower than the high-effort absorption case.
- [ ] Overlap outside the configured threshold must remain two independent zones.

## Acceptance Gate
P10.2 passes when:
1. It compiles cleanly.
2. Valid same-direction Breaker/FVG-family overlaps produce auditable cluster signals.
3. Cluster member provenance is correct.
4. Effort/result improves reaction discrimination without overriding P10 context.
5. Previous P10.1 regression cases remain valid.
