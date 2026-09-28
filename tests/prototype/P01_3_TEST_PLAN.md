# P01.3 Unified Participation — Test Plan

## Compile gate

- Pine Script v6 compiles with no errors.
- Script loads without plan-gated footprint requirements.
- Table renders without distorting price.

## State validation

Check that the unified state is plausible on confirmed bars:

- `NORMAL`: ordinary participation with no higher-priority event.
- `CONTRACTION`: RVOL below contraction threshold without stronger event.
- `COMPRESSION`: low effort plus low result after persistence.
- `EXPANSION_BULLISH` / `EXPANSION_BEARISH`: high effort, high directional result, directional close, and estimated-flow agreement when enabled.
- `VACUUM_BULLISH` / `VACUUM_BEARISH`: strong directional result despite low effort.
- `ABSORPTION_BULLISH`: strong estimated selling pressure with failed downside progress.
- `ABSORPTION_BEARISH`: strong estimated buying pressure with failed upside progress.
- `EXHAUSTION_BULLISH_MOVE`: extreme effort during an established up move with reduced directional efficiency.
- `EXHAUSTION_BEARISH_MOVE`: extreme effort during an established down move with reduced directional efficiency.
- `CLIMAX`: extreme RVOL/Z-score plus large range after persistence.

## Precedence validation

Verify higher-priority states suppress lower-priority states in this order:

1. unavailable
2. absorption
3. exhaustion
4. climax
5. liquidity vacuum
6. efficient expansion
7. compression
8. contraction
9. normal

## Score validation

Observe the table values around representative events:

- Effort Score should increase with RVOL and abnormal Z-score.
- Result Score should increase with large directional body/range and directional close.
- Flow Score should be +2 bullish, -2 bearish, 0 balanced/unavailable.
- Net Score should reflect candle direction plus flow contribution.

Scores are diagnostics, not trade signals.

## Cross-asset checks

Test at minimum:

- centralized-volume future
- equity
- crypto pair
- FX symbol using tick volume

Record whether thresholds behave consistently or require asset-class adaptation.

## Non-repainting check

- Live bar should display `FORMING`.
- Final state should only appear after bar confirmation.
- Historical states should remain unchanged after reload, subject to TradingView lower-timeframe data availability.

## Failure conditions

Flag for revision if:

- absorption fires repeatedly during ordinary trend continuation
- exhaustion duplicates climax too often
- expansion becomes rare because flow confirmation is too strict
- vacuum fires on ordinary low-volume bars without meaningful displacement
- contraction masks more informative states
- score ranges are not interpretable across asset classes
