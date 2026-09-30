# PLUTUS P08.2 — Unified Production Visual Test Plan

## Compile / Runtime
- Pine Script v6 compiles without errors.
- No runtime array bounds errors on empty zone/profile inventories.
- No object-limit errors during long chart runs.

## Display presets
- Minimal hides non-essential profile/zones/divergence visuals.
- Balanced shows core VWAP, POC/VAH/VAL, zones and status panel without histogram/divergence clutter.
- Full enables optional histogram/divergence/background controls.
- Diagnostic forces diagnostics visible.

## VWAP
- Active Session VWAP exists only while an ARTHA session is active.
- Daily, Weekly and Monthly VWAPs hard-break at their respective reset bars.
- No visual line joins across resets.

## Volume profile
- Histogram, POC, VAH and VAL all come from the same profile-row volume array.
- POC line aligns with the orange POC histogram row.
- Range Overlap is the default allocation.
- POC/VAH/VAL terminate at the selected profile-scope boundary.
- Session profiles disappear outside the selected session rather than extending indefinitely.
- Histogram fully disappears when toggled off.
- HVN/LVN are optional and use the same unified profile grid.

## Institutional zones
- FVG and OB candidates are typed correctly.
- IFVG only forms from an invalidated FVG after required follow-through.
- Breaker only forms from an invalidated OB after required follow-through.
- Duplicate/nested suppression works without array errors.
- Partial mitigation, retests, full mitigation, invalidation and expiry update lifecycle state.
- Zone boxes use `extend.none` and their right edge advances only to the current bar.
- Removed/expired/mitigated zones do not extend indefinitely.
- Hiding zones removes visuals without disabling internal scoring.

## Divergence
- Regular and hidden divergence lines are finite pivot-to-pivot segments.
- Maximum divergence object count is respected.
- Hiding divergence does not change confluence calculations.

## Confluence / status
- Score remains in [-100, 100].
- Confidence remains in [0, 100].
- Active/reliable engine counts are coherent.
- Compact and expanded panels render without changing logic.
- Status-panel position input works in all four positions.
- Diagnostics remain separate from production presentation.

## Signal gate
- No entry/exit trade signals are generated in P08.2.
- P09 remains the first signal-generation phase.
