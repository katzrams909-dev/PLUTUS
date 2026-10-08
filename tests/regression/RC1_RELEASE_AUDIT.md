# PLUTUS RC1 — Final Regression & Release Audit

Baseline under test: `pine/release/PLUTUS_RC1.pine`
Baseline commit: `c2e8d0860164f98b372038f177a92c0bf9d0bfae`

## Release policy

RC1 signal architecture is frozen during regression. A code change is permitted only to fix a demonstrated defect, lifecycle inconsistency, provenance error, repaint/runtime issue, or release-blocking performance problem. Any material trading-model change resets the affected regression cases.

Release requires:
1. clean Pine v6 compile,
2. no critical lifecycle/direction/provenance regression,
3. deterministic historical reload behavior,
4. acceptable runtime/object use,
5. all named regression cases explained by diagnostics,
6. no unresolved P0/P1 defects.

Severity:
- **P0** — compile/runtime/repaint/corrupt state/wrong-direction signal.
- **P1** — materially wrong signal classification, lifecycle, zone attribution, or hard-veto behavior.
- **P2** — scoring/threshold tuning or non-critical missed/extra signal.
- **P3** — visual/UX/documentation only.

---

## A. Baseline static audit

- [x] Pine v6 release file exists.
- [x] User-confirmed current RC1 compiles.
- [x] Main RC1 contains no `request.footprint()`.
- [x] Main RC1 contains no `request.security()` / HTF lookahead dependency.
- [x] Signal confirmation uses confirmed-bar gating.
- [x] FVG / IFVG / OB / Breaker use stable zone IDs.
- [x] Explicit reaction taxonomy exists:
  - FVG_REJECTION
  - FVG_SWEEP_RECLAIM
  - IFVG_RETEST
  - OB_REJECTION
  - OB_SWEEP_RECLAIM
  - BREAKER_RETEST
  - ZONE_FAILURE
  - CLUSTER_REJECTION
  - CLUSTER_SWEEP_RECLAIM
- [x] Trade-regime taxonomy exists:
  - TREND
  - COUNTERTREND
  - REVERSAL
- [x] P06 remains a capped confluence modifier; it is not a fourth P10 pillar.
- [x] Local divergence can strengthen institutional reactions.
- [x] Volume + effort/result + ADX/DI are reaction/regime qualifiers, not new top-level pillars.
- [x] Liquidity/reward-space gate exists.
- [x] Arrival-vs-reaction participation model exists.
- [x] Composite chop and ATR-volatility regime filters exist.
- [x] Final SQ is setup-centric.
- [x] Hard vetoes are evaluated separately from SQ.
- [x] Cluster mate selection no longer requires cluster score to exceed the primary standalone-zone score.
- [x] Dead `inversionConfirmBars` and `removeFullyMitigated` inputs removed.
- [ ] Long-history runtime stress passed.
- [ ] Object-limit stress passed.
- [ ] Historical reload determinism passed.

Note: `zPendingDir` remains in the lifecycle arrays. It is currently maintained consistently during creation, inversion, reset and removal. Treat it as legacy state for a later cleanup only after regression; do not remove it during RC1 testing unless it causes a demonstrated issue.

---

## B. Direction invariants — P0

For every tested signal:

- [ ] Bull FVG rejection/sweep never produces SHORT.
- [ ] Bear FVG rejection/sweep never produces LONG.
- [ ] Bull OB reaction never produces SHORT unless the source has explicitly failed/inverted.
- [ ] Bear OB reaction never produces LONG unless the source has explicitly failed/inverted.
- [ ] IFVG direction always follows the valid inverted FVG.
- [ ] Breaker direction always follows the valid failed OB.
- [ ] ZONE_FAILURE is the only direct opposite-source-direction failure class.
- [ ] Cluster direction equals both live member directions.
- [ ] A deleted/retired source cannot produce a later signal.
- [ ] Stable zone ID in label matches the actual interacted zone.

Any failure in this section blocks release.

---

## C. Zone lifecycle — P0/P1

### FVG
- [ ] Created only from qualified imbalance/displacement logic.
- [ ] Fresh -> Retested/Partial lifecycle correct.
- [ ] Sweep tolerance does not convert a decisive invalidation into a reclaim.
- [ ] Terminal full-fill bar may signal in original direction when otherwise qualified.
- [ ] Filled FVG retires on the following bar.
- [ ] Decisive single-bar far-side failure can create IFVG.
- [ ] Multi-bar drift through the gap does not create IFVG.

### IFVG
- [ ] Source FVG direction is discarded after valid inversion.
- [ ] JUST_FLIPPED state lasts only the intended transition bar.
- [ ] Later retest is evaluated in new direction.
- [ ] Full fill can be terminal then retires.
- [ ] Retired IFVG cannot signal.

### OB
- [ ] OB is tied to qualified displacement + structure break.
- [ ] First revisit has highest freshness.
- [ ] Retest count increments once per contact episode, not every touching bar.
- [ ] Mitigation depth decays inventory quality.
- [ ] Weak historical source cannot be rescued solely by SQ.
- [ ] Strong reaction can still qualify a moderate-but-valid source.
- [ ] Late-expansion weak-volume OB is suppressed when filters say it should be.
- [ ] A top-of-bull-trend bearish OB may classify COUNTERTREND or REVERSAL depending on evidence.
- [ ] The same bearish OB may later classify TREND after regime transition.

### Breaker
- [ ] Comes only from failed OB.
- [ ] Single-bar decisive failure is required.
- [ ] No multi-bar drift Breaker creation.
- [ ] First flipped retest is evaluated as BREAKER_RETEST.
- [ ] Breaker + aligned divergence receives local support.
- [ ] Breaker can classify REVERSAL before broad P10 bias fully flips.
- [ ] Later Breaker retest in established new regime can classify TREND.

---

## D. Reaction taxonomy — P1

### FVG_REJECTION
- [ ] Price enters/rebalances the FVG.
- [ ] Zone remains directionally valid.
- [ ] Confirmation closes/departs in FVG direction.
- [ ] Touch alone is insufficient.

### FVG_SWEEP_RECLAIM
- [ ] Price penetrates beyond the relevant boundary within allowed sweep tolerance.
- [ ] Price reclaims in original FVG direction.
- [ ] Decisive failure is not mislabeled sweep/reclaim.

### IFVG_RETEST
- [ ] Original FVG has already validly inverted.
- [ ] Interaction occurs after inversion.
- [ ] Retest reacts in flipped direction.

### OB_REJECTION
- [ ] Price mitigates the block.
- [ ] Reaction shows directional departure.
- [ ] Volume/ER/ADX-DI/context filters are evaluated.
- [ ] Poor late-expansion reaction is not accepted solely because the OB exists.

### OB_SWEEP_RECLAIM
- [ ] Probe/sweep is within allowed semantics.
- [ ] Block is reclaimed.
- [ ] Departure/participation supports the intended direction.

### BREAKER_RETEST
- [ ] Original OB has failed.
- [ ] Breaker exists in flipped direction.
- [ ] Retest is subsequent, not arbitrary overlap.
- [ ] Reaction confirms flipped direction.

### Cluster reactions
- [ ] Cluster uses same-direction Breaker + FVG/IFVG only.
- [ ] Intersection is operative interaction range when enabled.
- [ ] Both members are live at arming.
- [ ] Retirement/invalidation of a member invalidates armed cluster.
- [ ] No duplicate standalone + cluster signal from the same event inside re-entry controls.

---

## E. Trend / Countertrend / Reversal classifier — P1

### TREND
- [ ] Setup direction agrees with prevailing regime.
- [ ] P10 directional context is aligned.
- [ ] ADX/DI support is sensible for continuation.
- [ ] No severe opposing divergence/context veto.
- [ ] Trend threshold is lower than countertrend/reversal by design.

### COUNTERTREND
- [ ] Setup opposes a non-neutral prevailing regime.
- [ ] Requires higher setup quality.
- [ ] Requires stronger absorption.
- [ ] Requires at least one meaningful advantage: extreme location, aligned divergence, liquidity sweep, cluster, or OB sweep/reclaim.
- [ ] Does not silently become TREND merely because confluence temporarily goes neutral.

### REVERSAL
- [ ] Setup opposes prevailing trend.
- [ ] Institutional source requirement works.
- [ ] Requires actual reversal evidence: flip/failure, sweep/reclaim, extreme + divergence, divergence + deterioration, or aligned liquidity sweep.
- [ ] Can confirm before full broad-bias flip.
- [ ] Once broad regime transitions, subsequent same-direction setups classify TREND.
- [ ] Reversal classification has priority over countertrend when reversal evidence is satisfied.

---

## F. Participation / volume / effort-result — P1/P2

- [ ] Arrival pressure uses completed bars before interaction.
- [ ] Reaction pressure includes current/recent reaction bars.
- [ ] Strong opposing arrival + weak response blocks weak institutional reactions.
- [ ] Strong opposing arrival + high absorption may still qualify.
- [ ] Reaction dominance increases candidate quality/rank.
- [ ] Weak reaction RVOL penalizes OB/BB.
- [ ] Strong expanding directional RVOL supports OB/BB.
- [ ] High volume continuing *through* a zone is not mistaken for absorption.
- [ ] FX/CFD volume is treated as relative participation, not centralized traded volume.

Regression examples:
- [ ] weak bullish OB bounce after mature bullish expansion is filtered/downgraded.
- [ ] strong sell arrival into bullish OB that fails to make downside progress and reclaims can still qualify LONG.

---

## G. ADX / DI and market state — P1/P2

- [ ] Strong aligned ADX + DI supports trend continuation.
- [ ] Strong opposing DI blocks/penalizes weak OB/BB reaction.
- [ ] ADX rollover after extension contributes to exhaustion evidence.
- [ ] DI cross alone never creates a signal.
- [ ] Low ADX + weak confluence + high overlap identifies chop.
- [ ] ATR compression strengthens chop classification.
- [ ] Ordinary reactions are suppressed in composite chop.
- [ ] Sweep/reclaim and qualified failure exceptions work in chop.
- [ ] Extreme ATR regime requires higher live reaction quality.
- [ ] Qualified reversal exception can operate in extreme volatility.
- [ ] Volatility filter does not eliminate all signals around active-session expansions.

---

## H. Liquidity and reward-space — P1

- [ ] Bullish reversal/countertrend can use confirmed sell-side swing sweep evidence.
- [ ] Bearish reversal/countertrend can use confirmed buy-side swing sweep evidence.
- [ ] Nearest live opposing institutional inventory ahead is detected.
- [ ] Relevant swing-liquidity target ahead is detected.
- [ ] Nearest obstacle is selected correctly.
- [ ] Buffer is applied in correct direction.
- [ ] Available reward space is measured against prospective structural risk.
- [ ] Setup below minimum available R is blocked with NO_SPACE.
- [ ] No obstacle ahead does not create an artificial block.
- [ ] Opposing zone behind entry is ignored.

---

## I. Final SQ — P1/P2

SQ components:
- Source/setup
- Reaction
- Participation/flow
- Regime
- Context/confidence
- Reward space

Checks:
- [ ] Component weights normalize correctly even when changed.
- [ ] Zero/near-zero total configured weight cannot divide by zero.
- [ ] Trend / Countertrend / Reversal final thresholds apply correctly.
- [ ] Hard invalid zone cannot be rescued by high SQ.
- [ ] NO_SPACE cannot be rescued by high SQ.
- [ ] CHOP/HIGH_VOL veto cannot be rescued by high SQ.
- [ ] Wrong institutional context cannot be rescued by high SQ.
- [ ] Final label SQ equals frozen confirmation SQ.
- [ ] Historical labels do not recalculate to newer context.
- [ ] SQ remains finite and never NA on confirmed signals.

---

## J. Veto diagnostics — P1/P3

Expected codes:
- NOT_ARMED
- ZONE_INVALID
- WAIT_NEXT_BAR
- SESSION
- RISK
- NO_SPACE
- CHOP
- HIGH_VOL
- REVERSAL_EVIDENCE
- COUNTERTREND_EVIDENCE
- CONTEXT
- EFFORT_RESULT
- OB_REACTION
- BREAKER_REACTION
- INST_CONTEXT
- FLOW_ADX
- WAIT_CONFIRM
- SQ
- READY
- CONFIRMED

Checks:
- [ ] The first blocking reason shown is the actual controlling gate.
- [ ] READY means all hard gates pass and confirmation/SQ state is coherent.
- [ ] CONFIRMED persists appropriately for the just-confirmed setup.
- [ ] Diagnostics never alter signal calculations.
- [ ] Diagnostic mode can explain every named regression screenshot.

---

## K. Named regression cases

### XAUUSD Breaker + divergence
Target behavior:
`Bear Breaker retest -> bearish reaction/displacement -> aligned bearish divergence -> SHORT`

- [ ] Breaker provenance correct.
- [ ] Bearish divergence locally strengthens setup.
- [ ] Not blocked merely because inherited Breaker score is modest.
- [ ] Reaction/volume/ADX context remains valid.
- [ ] Regime classification is reasonable: REVERSAL, COUNTERTREND, or TREND according to prevailing state.
- [ ] Entry is not materially delayed beyond the intended confirmation close.

### GBPUSD blue-box false bullish OB
Target behavior:
`mature bullish expansion -> weak/late bullish OB reaction -> poor participation/regime/location -> NO LONG`

- [ ] Arrival vs reaction flow is visible in diagnostics.
- [ ] ADX/DI condition is visible.
- [ ] Extension/location is visible.
- [ ] If blocked, veto reason is explainable and stable.
- [ ] If still allowed, record as P1 regression with screenshot/bar time and inspect exact gate.

### SPX500 missed bullish OB reactions
- [ ] Strong valid first-touch OB reaction can qualify.
- [ ] Moderate source score does not automatically block exceptional reaction.
- [ ] FVG competition does not steal attribution solely through static zone score.
- [ ] Correct OB provenance is retained.

---

## L. Non-repainting / determinism — P0

For at least 20 confirmed signals across multiple instruments:
- [ ] record timestamp, regime, reaction, zone ID, entry, SQ.
- [ ] reload chart.
- [ ] all recorded signals remain on same confirmed bar.
- [ ] regime label unchanged.
- [ ] reaction label unchanged.
- [ ] zone ID/provenance unchanged.
- [ ] entry/stop/target unchanged.
- [ ] SQ unchanged.
- [ ] no historical label moves backward.
- [ ] no pivot/divergence event appears before its confirmation became available.

---

## M. Runtime / object-limit stress — P0

Test with Full/Diagnostic display and maximum practical history:
- [ ] maxTrackedZones = 30 default.
- [ ] maxTrackedZones = 80 stress.
- [ ] histogram enabled.
- [ ] session boxes enabled.
- [ ] divergence enabled.
- [ ] transitions enabled.
- [ ] signal levels/prices/risk-reward enabled.
- [ ] no array-bounds errors.
- [ ] no loop-time/runtime timeout.
- [ ] no box/line/label limit error.
- [ ] hiding zones deletes visuals without affecting calculations.
- [ ] repeated array removal preserves ID/provenance alignment.
- [ ] O(n²) cluster scan remains usable at max zone count.

---

## N. Historical market matrix

Minimum:
| Market | TF | Period | Trend | Range | Reversal | Status |
|---|---:|---|:---:|:---:|:---:|---|
| XAUUSD | 15m | 3–6 months | [ ] | [ ] | [ ] | |
| XAUUSD | 1h | 6–12 months | [ ] | [ ] | [ ] | |
| NAS100 | 5m | 2–3 months | [ ] | [ ] | [ ] | |
| NAS100 | 15m | 3–6 months | [ ] | [ ] | [ ] | |
| SPX500 | 5m | 2–3 months | [ ] | [ ] | [ ] | |
| SPX500 | 15m | 3–6 months | [ ] | [ ] | [ ] | |
| GBPUSD | 15m | 3–6 months | [ ] | [ ] | [ ] | |
| GBPUSD | 1h | 6–12 months | [ ] | [ ] | [ ] | |
| EURUSD | 15m | 3–6 months | [ ] | [ ] | [ ] | |
| USDJPY | 15m | 3–6 months | [ ] | [ ] | [ ] | |
| USDCAD | 15m | 3–6 months | [ ] | [ ] | [ ] | |
| BTCUSD | 15m | optional | [ ] | [ ] | [ ] | |

For each test period record:
- total signals,
- TREND / COUNTERTREND / REVERSAL counts,
- FVG / IFVG / OB / BREAKER / CLUSTER reaction counts,
- unexplained false positives,
- missed high-quality reactions,
- stale-zone signals,
- duplicate signals,
- NO_SPACE blocks,
- CHOP/HIGH_VOL blocks,
- obvious late entries.

---

## O. Release blocker log

| ID | Severity | Instrument / TF | Bar time | Expected | Actual | Veto / SQ | Status |
|---|---|---|---|---|---|---|---|
| REG-XAU-BB-DIV | P1 | XAUUSD / 1h | screenshot case | Bear Breaker + DIV short | pending retest | | OPEN |
| REG-GBP-OB-BLUE | P1 | GBPUSD / screenshot TF | blue-box case | suppress weak late Bull OB long | pending retest | | OPEN |
| REG-SPX-OB | P1 | SPX500 / screenshot TF | marked cases | recover strong Bull OB reactions | pending retest | | OPEN |

Add new cases rather than changing rules from one screenshot without checking the wider matrix.

---

## P. v1.0 gate

PLUTUS v1.0 may be tagged only when:

- [x] current RC1 compiles cleanly.
- [ ] all P0 sections pass.
- [ ] named XAUUSD/GBPUSD/SPX500 cases are rechecked.
- [ ] no open P0 defect.
- [ ] no unexplained P1 wrong-direction/lifecycle/provenance defect.
- [ ] reload determinism passes.
- [ ] runtime/object-limit stress passes.
- [ ] at least the minimum historical matrix is reviewed.
- [ ] documentation matches implemented logic.
- [ ] final RC commit SHA is recorded.
- [ ] RC is frozen and tagged as v1.0.

Current status: **RC1 FEATURE-FROZEN / COMPILE-PASS / FINAL REGRESSION RECOMMENDED**.


---

## Q. P10.4 Reaction State Machine Regression

Baseline reaction-state commits:
- P10.3: `a4808d7adf8ac3a7102a616350da87aa5cfe7fc4`
- RC1: `ed840b27a02ff7e4a15351ef7aa5d2cf78368568`

### Wick interaction
- [ ] Wick-only overlap with FVG is recognized as interaction.
- [ ] Wick-only overlap with OB is recognized as interaction.
- [ ] Wick-only overlap with IFVG is recognized only on a later retest bar.
- [ ] Wick-only overlap with Breaker is recognized only on a later retest bar.
- [ ] Wick enters Bull zone + closes above zone can confirm bullish rejection.
- [ ] Wick enters Bear zone + closes below zone can confirm bearish rejection.
- [ ] Mitigation-basis setting does not make signal wick interaction invisible.

### 1 / 2 / 3-bar reaction window
- [ ] 1-bar immediate reaction confirms on interaction-bar close.
- [ ] Bar 1 closes inside + Bar 2 reclaims correctly -> confirms on Bar 2.
- [ ] Bar 1 inside + Bar 2 remains valid + Bar 3 reclaims -> confirms on Bar 3.
- [ ] Bar 4 reclaim with 3-bar maximum -> rejected as expired.
- [ ] Earliest valid confirmation wins.
- [ ] Reaction deadline does not reset merely because price remains inside the same zone.
- [ ] Pending setup invalidates immediately if the bound zone invalidates/retires.

### Reclaim modes
- [ ] Outside Zone requires Bull close > zoneTop / Bear close < zoneBottom.
- [ ] Midpoint requires directional reclaim through zone midpoint.
- [ ] Directional Close follows configured directional-close quality.
- [ ] Directional-departure requirement works independently of reclaim mode.

### IFVG / Breaker post-flip invariant
- [ ] Flip candle creates IFVG/Breaker but cannot signal.
- [ ] Flip candle is never counted as the retest.
- [ ] Earliest eligible interaction is bar strictly after flip bar.
- [ ] Later 1-bar wick/body retest can confirm.
- [ ] Later 2-bar retest can confirm.
- [ ] Later 3-bar retest can confirm.
- [ ] No same-candle ZONE_FAILURE entry leaks through the flipped-zone path.

### Multi-bar evidence retention
- [ ] Best absorption from Bar A/B is available to Bar B/C confirmation.
- [ ] Best reaction quality within the pending window is retained.
- [ ] Best RVOL within the pending window contributes to final flow quality.
- [ ] Aligned divergence discovered within the short pending window is retained.
- [ ] Evidence after reaction expiry cannot retroactively qualify the old setup.

### Regime / labels
- [ ] Initial regime is retained for diagnostics.
- [ ] Final regime is classified on confirmation bar.
- [ ] Countertrend can upgrade to Reversal during a valid multi-bar sequence.
- [ ] Detailed label shows 1B / 2B / 3B.
- [ ] Detailed label adds WICK when a wick-only interaction occurred.
- [ ] Entry remains on actual confirmation-bar close, never back-plotted to first touch.

### Screenshot regression cases
- [ ] NAS100 1m circled two-stage bullish reactions are rechecked.
- [ ] NAS100 15m delayed reclaim case is rechecked.
- [ ] SPX500 15m delayed bullish reclaim is rechecked.
- [ ] AUDUSD 1h bearish wick/retest example is rechecked.
- [ ] Prior GBPUSD blue-box false long remains absent after the reaction-state change.

Any same-flip-bar IFVG/Breaker signal or back-plotted delayed entry is a P0 release blocker.


### P10.4 compile checkpoint
- [x] User compile confirmation for P10.4 RC1 `ed840b27a02ff7e4a15351ef7aa5d2cf78368568`.
- [ ] Recheck named multi-bar/wick regression cases on chart.
- [ ] Confirm no same-flip-bar IFVG/Breaker signals.
- [ ] Confirm prior GBPUSD false-long regression remains closed.


---

## Release Freeze Record

- [x] Trading-model feature freeze accepted.
- [x] Frozen RC baseline: `c2e8d0860164f98b372038f177a92c0bf9d0bfae`.
- [x] User-confirmed Pine v6 compile pass at frozen baseline.
- [x] Release notes added in `docs/RELEASE_v1.0.md`.
- [x] README / project specification / engine architecture updated for v1.0.
- [ ] Git tag `v1.0` created.
- [ ] Optional extended historical/runtime regression completed.

Policy after freeze: only demonstrated compile/runtime, repaint, direction, lifecycle, provenance, or release-blocking defects should change the frozen model.
