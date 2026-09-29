# PLUTUS P05.2 — Zone Lifecycle Test Plan

## Objective
Validate refined lifecycle behavior for qualified FVG, OB, IFVG and Breaker zones without introducing repainting, unbounded objects or stale visual artifacts.

## Compile gate
- Pine Script v6 compiles without errors.
- No global-variable mutation errors from helper functions.
- No array-size or object-limit runtime errors.

## Candidate creation
- Bullish/bearish FVG candidates require displacement and minimum gap width.
- Bullish/bearish OB candidates require opposite candle + displacement break.
- Birth score is bounded to 0–100.
- `Show Only Qualified Zones` suppresses unqualified drawings without suppressing internal tracking.

## Mitigation progression
Test both `Wick` and `Close` mitigation basis.

Expected lifecycle:
1. Fresh zone starts at 0% mitigation.
2. Shallow penetration below the partial threshold remains fresh.
3. Penetration beyond the partial threshold changes status to partial.
4. Deeper penetration increases stored mitigation monotonically; it must never decrease.
5. Penetration beyond the full threshold changes status to fully mitigated.
6. If `Remove Fully Mitigated Zones` is enabled, the box stops and disappears at that bar.
7. If disabled, the box stops extending and remains visibly historical.

## Retests
- First re-entry after leaving a zone increments retest count once.
- Consecutive bars remaining inside a zone do not increment retest count every bar.
- Leaving and re-entering increments the count again.
- Retest penalty reduces current score but never below zero.

## Freshness decay
- Score decays with zone age according to `Freshness Decay Per 100 Bars`.
- Birth score remains unchanged for auditability.
- Current score remains bounded 0–100.

## Invalidation
- Bullish zone invalidates only when close is below the zone bottom.
- Bearish zone invalidates only when close is above the zone top.
- Invalidated drawings stop extending immediately.
- Removal option behaves correctly.

## FVG -> IFVG
With `Create IFVG From Invalidated FVG` enabled:
- Invalidated FVG creates an opposite-direction IFVG candidate.
- IFVG inherits reduced mechanical quality using `Inherited Mechanical Quality`.
- IFVG is re-qualified using current P01–P04 directional context.
- IFVG is only created when score >= `Minimum Inversion Qualification`.
- No repeated IFVG is created from the same already-invalidated parent.

## OB -> Breaker
With `Create Breaker From Invalidated OB` enabled:
- Invalidated OB creates an opposite-direction Breaker candidate.
- Breaker is re-qualified in the new direction.
- Breaker must not simply inherit the original OB qualification score.
- No repeated Breaker is created from the same already-invalidated parent.

## Expiry
- Active zones expire after `Maximum Zone Age`.
- Expired zones stop extending.
- Removal option behaves correctly.

## Object discipline
- `Maximum Tracked Zones` bounds all zone arrays.
- Oldest zone object is deleted when tracking limit is exceeded.
- Histogram uses a fixed reusable pool.
- No unbounded box creation from histogram updates.

## VWAP reset rendering
For each VWAP reference:
- Active Session
- Daily
- Weekly
- Monthly

Confirm the plotted VWAP contains a visible hard break at its reset boundary and does not vertically join the old and new period.

## Profile reset rendering
For each profile scope:
- Asia
- London
- New York
- Day

Confirm POC / VAH / VAL do not join across the profile reset boundary.

## Histogram toggle
- `Show Volume Profile Histogram = ON`: histogram renders to the right of price.
- `OFF`: histogram disappears completely.
- Toggle repeatedly while chart is running; no stale boxes remain.

## Diagnostics
Confirm table values update correctly for:
- fresh zones
- partial zones
- mitigated zones
- invalidated zones
- expired zones
- active IFVGs
- active Breakers
- highest active score
- maximum active mitigation
- P01/P02/P03/P04 directional context
- histogram state
- tracked zone count

## Cross-asset sanity
Run at minimum on:
- XAUUSD
- one FX pair
- one equity index
- one crypto pair

Use multiple intraday timeframes. Verify no runaway object growth and no obvious lifecycle state oscillation caused solely by zoom/timeframe changes.
