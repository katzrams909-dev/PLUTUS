# P03.4 — Unified Volume Profile Test Plan

## Purpose
Validate the consolidated P03 engine and the bounded horizontal histogram before P03 is considered prototype-complete.

## Compile
- Pine Script v6 compiles without errors.
- No max box / label / plot warnings during normal use.

## Profile Core
- Test Asia, London, New York, and Day scopes.
- Confirm selected scope resets cleanly using the shared timezone.
- Confirm profile storage remains bounded by Maximum Bars Per Profile.
- Confirm POC / VAH / VAL remain consistent with P03.1 behavior.

## Histogram
- Histogram is visible only when Show Current Profile Histogram is enabled and the selected scope is active.
- Histogram sits to the right of current price bars and does not overlap unexpectedly at default settings.
- Maximum histogram width changes correctly with Maximum Width (Bars).
- Right Offset moves the entire histogram without changing relative bin widths.
- POC bin is visually emphasized when enabled.
- Value-area bins are visually distinguished when enabled.
- Histogram widths scale relative to the largest current bin.
- All histogram boxes are reused rather than accumulated continuously.
- Turning the histogram off hides all boxes cleanly.
- Session/profile reset does not leave old histogram boxes behind.

## HVN / LVN
- Strongest HVN / LVN update from the same current-profile bin distribution.
- Candidate counts react appropriately to smoothing and threshold changes.
- Node proximity score rises as price approaches the nearest qualifying node.
- Node plotting can be disabled without affecting state logic.

## Migration
- Rising POC + rising VAH/VAL produces positive migration context.
- Falling POC + falling VAH/VAL produces negative migration context.
- Value-area expansion / contraction behaves consistently with P03.3.
- Persistence logic does not flip rapidly under minor one-bar changes.
- Current POC vs previous completed POC comparison survives scope reset.

## Unified Scores
- Migration Score stays within [-100, +100].
- Location Score maps correctly from BELOW_VALUE through ABOVE_VALUE.
- Node Proximity stays within [0, 100].
- Profile Direction stays within [-100, +100].

## Unified States
Exercise and visually verify, where available:
- PROFILE_UNAVAILABLE
- PROFILE_CONFLICT
- PROFILE_ACCEPTED_HIGHER
- PROFILE_ACCEPTED_LOWER
- PROFILE_EXTENDED_ABOVE
- PROFILE_EXTENDED_BELOW
- PROFILE_LVN_TRANSITION
- PROFILE_HVN_ACCEPTANCE
- PROFILE_MIGRATING_HIGHER_EXPANDING
- PROFILE_MIGRATING_LOWER_EXPANDING
- PROFILE_MIGRATING_HIGHER
- PROFILE_MIGRATING_LOWER
- PROFILE_VALUE_EXPANDING
- PROFILE_VALUE_CONTRACTING
- PROFILE_LEANING_HIGHER
- PROFILE_LEANING_LOWER
- PROFILE_BALANCED
- PROFILE_TRANSITION
- FORMING on unconfirmed realtime bar

## Visual / Performance
- Default chart remains readable.
- No historical histogram accumulation.
- No indefinite line extension from developing levels after scope closes.
- Histogram remains responsive with 24 default bins.
- Test 80 bins and Maximum Bars Per Profile at 1000 for stress behavior.
- Test on XAUUSD and at least one FX, index, or crypto instrument if available.

## Gate
P03.4 passes when it compiles cleanly, histogram rendering is correct, resets are clean, box usage is bounded, and unified states/scores behave plausibly across multiple profile conditions.
