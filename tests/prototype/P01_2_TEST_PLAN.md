# P01.2 — Footprint Enhancement Test Plan

## Purpose

Validate that native footprint data improves P01 participation interpretation without breaking the universal volume core.

## Compile / access gate

1. Confirm Pine v6 compiles.
2. Confirm the account supports `request.footprint()`.
3. Confirm the symbol/timeframe returns non-na footprint data.
4. If footprint data is unavailable for a bar, verify the table reports `Footprint: N/A` and the universal RVOL/effort-result diagnostics remain conceptually valid.

## Footprint diagnostics

Check that the table reports plausible values for:

- buy volume
- sell volume
- total delta
- delta percentage
- row count
- buy imbalance density
- sell imbalance density
- POC
- VAH
- VAL
- footprint bias

Sanity checks:

- buy volume + sell volume should approximately equal footprint total volume internally.
- positive delta should correspond to buy volume > sell volume.
- negative delta should correspond to sell volume > buy volume.
- POC should lie within the bar's footprint range.
- VAL <= POC <= VAH under normal footprint construction.

## Directional interpretation

### Expansion

Expected:
- strong bullish price result + high effort + positive/dominant footprint evidence can confirm bullish expansion.
- strong bearish price result + high effort + negative/dominant footprint evidence can confirm bearish expansion.

Flag if:
- expansion confirms while footprint pressure strongly opposes price direction.

### Bullish absorption

Expected candidate behavior:
- strong negative delta / sell pressure,
- elevated effort,
- weak downside progress or a close that recovers into the upper half of the bar.

Interpretation: aggressive selling is not producing proportional downside result.

### Bearish absorption

Expected candidate behavior:
- strong positive delta / buy pressure,
- elevated effort,
- weak upside progress or a close that falls into the lower half of the bar.

Interpretation: aggressive buying is not producing proportional upside result.

## Persistence

Default Confirmation Bars = 2.

Verify:
- a single candidate bar does not immediately produce a confirmed state when persistence = 2.
- two consecutive qualifying bars do confirm.
- increasing persistence reduces frequency materially.

## Instruments

Prefer testing on:

1. centralized-volume futures instrument,
2. liquid equity,
3. liquid crypto market,
4. forex symbol for comparison where footprint availability/behavior may differ.

## Prototype acceptance criteria

P01.2 passes if:

- it compiles and runs on an eligible account,
- footprint values are internally coherent,
- footprint-enhanced absorption appears selectively rather than continuously,
- footprint-confirmed expansion agrees with visible directional participation most of the time,
- universal P01 diagnostics remain understandable alongside footprint data,
- no chart clutter is introduced with default settings.
