# P04.2 Test Plan — Auction Acceptance / Rejection Strength

## Compile gate
- Pine Script v6 compiles without errors.

## Profile mechanics
- Validate Asia, London, New York, and Day scopes.
- Validate POC / VAH / VAL reset cleanly on scope change.
- Validate previous completed VAH / VAL snapshot on the first bar of a new scope.

## Acceptance strength
- Sustained closes above VAH should raise signed acceptance positively.
- Sustained closes below VAL should drive signed acceptance negatively.
- Sustained closes inside value should raise overall acceptance strength without necessarily creating directional bias.

## Rejection strength
- Excursion above VAH followed by a close back inside value should produce negative rejection strength.
- Excursion below VAL followed by a close back inside value should produce positive rejection strength.
- Larger ATR-normalized excursions should increase rejection magnitude up to the bounded limit.

## Rotation strength
- Repeated POC crossings should increase rotation strength.
- Directional auctions with few POC crossings should keep rotation strength comparatively low.

## Directional strength
- Increasing distance above POC should move directional strength toward +100.
- Increasing distance below POC should move directional strength toward -100.
- Values must remain bounded to [-100, +100].

## Displacement strength
- Current value midpoint above previous completed value midpoint should be positive.
- Current midpoint below previous should be negative.
- Values must remain bounded to [-100, +100].

## Aggregate scores
- Bullish acceptance + positive distance + positive displacement should raise imbalance score.
- Bearish acceptance + negative distance + negative displacement should lower imbalance score.
- Strong inside-value acceptance and repeated POC rotation should raise balance score.
- Balance score must stay within [0, 100].

## State validation
Confirm representative occurrences of:
- AUCTION_STRONGLY_BULLISH
- AUCTION_STRONGLY_BEARISH
- AUCTION_BULLISH
- AUCTION_BEARISH
- AUCTION_BALANCED
- AUCTION_REJECTION_BULLISH
- AUCTION_REJECTION_BEARISH
- AUCTION_TRANSITION
- AUCTION_UNAVAILABLE
- FORMING on an unconfirmed live bar

## Visual / object behavior
- No bridged POC / VAH / VAL lines across reset boundaries.
- Previous levels only display when enabled.
- Diagnostic table values match visible chart behavior.

## Suggested instruments
- XAUUSD
- major FX pair
- index CFD/future where volume is usable
- crypto pair where volume is available
