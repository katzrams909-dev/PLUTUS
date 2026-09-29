# PLUTUS P05.0 — Zone Candidate Framework Test Plan

## Objective
Validate candidate detection, scoring, lifecycle management, breaker conversion, and object discipline for the first Institutional Zones prototype.

## Compile gate
- Pine Script v6 compiles without errors.
- No runtime object-limit errors under default settings.

## Candidate detection
1. Confirm bullish FVG candidates appear only when `low > high[2]` and minimum ATR width is satisfied.
2. Confirm bearish FVG candidates appear only when `high < low[2]` and minimum ATR width is satisfied.
3. Confirm bullish OB candidates require a bearish prior candle plus bullish displacement closing through prior high.
4. Confirm bearish OB candidates require a bullish prior candle plus bearish displacement closing through prior low.
5. Raise `Minimum Displacement Body / ATR` and verify fewer OB candidates.
6. Raise `Minimum FVG Width / ATR` and verify fewer FVG candidates.

## Qualification score
- Score remains within 0–100.
- Increasing current-bar RVOL should increase the participation component.
- Larger efficient displacement should increase displacement/efficiency components.
- Very wide zones should receive less width-quality credit.
- `Show Only Qualified Zones` must not change lifecycle logic or counts; it is display-only.

## Lifecycle
1. Fresh zone begins with status 0.
2. First later touch changes it to mitigated and applies the configured score penalty once.
3. Bullish zone invalidates when a confirmed close is below its bottom.
4. Bearish zone invalidates when a confirmed close is above its top.
5. Invalidated zones disappear when `Remove Invalidated Zones` is enabled.
6. Zones older than `Maximum Zone Age` expire.
7. Expired zones disappear when `Remove Expired Zones` is enabled.

## Breaker conversion
- With breaker detection enabled, invalidating a bullish OB creates a bearish breaker using the same price bounds.
- Invalidating a bearish OB creates a bullish breaker.
- Breaker score equals prior OB score multiplied by `Breaker Score Factor`, bounded to 0–100.
- FVG invalidation must not create a breaker.
- Breaker invalidation must not recursively create another breaker.

## Object discipline
- Tracked records never exceed `Maximum Tracked Zones`.
- Oldest tracked records are deleted when the cap is exceeded.
- Hidden candidates do not accumulate additional display objects.
- Repeated invalidation/expiry does not produce orphaned boxes.

## Visual QA
- Bullish and bearish zones are distinguishable.
- Qualified zones are visually stronger than unqualified candidates.
- Optional zone text shows type, direction and score.
- Mitigated zones are visually de-emphasized.

## Architectural limitation / next stage
P05.0 uses local displacement, RVOL, efficiency and zone-width evidence as a qualification baseline. P05.1+ should feed direct P01 Participation, P02 Fair Value, P03 Profile and P04 Value Analysis outputs into the institutional qualification model rather than duplicating those engines.
