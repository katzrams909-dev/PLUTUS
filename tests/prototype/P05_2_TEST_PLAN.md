# PLUTUS P05.2 — Refined Zone Lifecycle Test Plan

## Objective
Validate that FVG, OB, IFVG and Breaker zones are logically formed, lifecycle-safe, non-duplicative and visually bounded.

## Compile gate
- Pine Script v6 compiles without errors.
- No array-size or object-limit runtime errors.

## FVG semantic quality
- FVG requires a real 3-candle imbalance.
- The middle candle must be directional displacement.
- Middle body / ATR must meet `Minimum Displacement Body / ATR`.
- Middle body / range must meet `Minimum Displacement Body / Range`.
- Gap width must remain within configured ATR bounds.
- Weak, doji-like and low-efficiency middle candles must not create FVG boxes.

## OB semantic quality
- Bullish OB must be an opposing bearish candle immediately before bullish displacement.
- Bearish OB must be an opposing bullish candle immediately before bearish displacement.
- Displacement must close through recent structure defined by `OB Structure Break Lookback`.
- Test both `Body` and `Full Candle` OB range modes.
- Over-wide OBs must be rejected.

## Duplicate / nested suppression
- Same-type, same-direction zones with excessive overlap must not stack duplicate boxes.
- Zones closer than `Minimum Same-Type Zone Separation / ATR` must be suppressed.
- Legitimately separate zones must still be retained.

## Mitigation progression
Test both `Wick` and `Close` mitigation basis.
1. Fresh zone begins at 0% mitigation.
2. Partial penetration updates mitigation monotonically.
3. Full mitigation stops the box.
4. Removal settings delete the box when enabled.
5. Retest count increases only on a new re-entry, not every consecutive inside bar.

## Invalidation
- Bullish zone invalidates on a close below its bottom.
- Bearish zone invalidates on a close above its top.
- Invalidated boxes stop immediately.

## FVG -> IFVG
- Only an original FVG can arm IFVG conversion.
- Close through the parent zone must exceed `Minimum Close Through Zone / ATR`.
- IFVG must not appear on the same invalidation bar.
- A later directional follow-through bar must hold beyond the former zone within `Inversion Confirmation Window`.
- Failed or timed-out inversion candidates must be discarded.
- IFVG must re-qualify above `Minimum Inversion Qualification`.
- IFVG cannot recursively create another IFVG.

## OB -> Breaker
- Only an original OB can arm Breaker conversion.
- Break must be decisive and directionally aligned.
- Breaker requires later follow-through confirmation.
- Breaker is re-qualified in its new direction.
- Breaker cannot recursively produce another Breaker.

## Freshness / expiry
- Age, mitigation and retests reduce current score but not birth score.
- Expired zones stop extending at `Maximum Zone Age`.

## VWAP / profile reset rendering
- Active Session, Daily, Weekly and Monthly VWAPs visibly break at reset.
- Asia, London, New York and Day profile POC/VAH/VAL visibly break at reset.

## Histogram
- `Show Volume Profile Histogram` toggles the entire histogram on/off.
- OFF leaves no stale boxes.
- `Histogram Row Size (Profile Bins)` = 1 shows full profile-bin resolution.
- Larger row size aggregates adjacent bins into taller/coarser rows.
- Test row sizes 1, 2, 4 and 8.
- POC/value-area highlighting remains coherent after aggregation.

## Object discipline
- `Maximum Tracked Zones` bounds all zone arrays.
- Histogram uses a fixed reusable box pool.
- No runaway object growth after repeated zone creation/invalidation.

## Cross-asset sanity
Test at minimum:
- XAUUSD
- one FX pair
- one equity index
- one crypto pair

Review multiple intraday timeframes specifically for visually illogical or duplicate FVG/OB/IFVG/Breaker boxes.
