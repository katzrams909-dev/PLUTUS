# PLUTUS P03.2 — HVN / LVN Detection Test Plan

## Compile gate
- Pine Script v6 compiles without errors.

## Baseline profile checks
- POC / VAH / VAL remain consistent with P03.1 for the same inputs.
- Scope reset behavior remains correct for Asia, London, New York, and Day.
- No profile levels bridge across a reset bar.

## HVN checks
- HVN candidates only appear at local maxima of the smoothed bin-volume curve.
- Candidate relative volume must meet `HVN Min Volume / Mean`.
- Strongest HVN is the highest qualifying strength score.
- Second HVN, when enabled, respects minimum bin separation from the strongest HVN.

## LVN checks
- LVN candidates only appear at local minima of the smoothed bin-volume curve.
- Candidate relative volume must meet `LVN Max Volume / Mean`.
- Strongest LVN is the deepest qualifying low-volume node.
- Second LVN, when enabled, respects minimum bin separation from the strongest LVN.

## Smoothing checks
- Smoothing Radius = 0 uses raw bin volume.
- Increasing smoothing reduces isolated one-bin spikes/troughs rather than creating new extreme nodes mechanically.

## Visual/object checks
- No persistent line objects are created.
- Node plots exist only during the active profile scope.
- At most two HVNs and two LVNs are plotted.
- Disabling `Show Strongest HVN / LVN Nodes` removes node visuals without altering profile calculations.

## Diagnostics
Confirm the table reports:
- scope / scope state
- stored bars / bins
- VAH / POC / VAL
- price location
- mean smoothed bin volume
- HVN candidate count and strongest/second node
- LVN candidate count and strongest/second node
- strength ratios

## Expected approximation limitation
As with P03.0/P03.1, each chart bar contributes its full reported volume to one allocation-price bin. P03.2 detects nodes from that approximate distribution; it does not claim exchange-grade tick-by-price volume.
