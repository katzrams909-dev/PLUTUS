# P02.0 — VWAP Baseline Test Plan

## Objective
Validate reset mechanics, session containment, and visual continuity for the PLUTUS VWAP baseline before adding interpretation logic.

## Compile / render
- Pine Script v6 compiles without warnings/errors.
- Indicator renders on the price chart.
- Table remains readable.

## Session VWAP
- Session VWAP is visible only while the configured session is active.
- Session VWAP starts from the first in-session bar.
- Session VWAP does not continue through the closed period.
- A new session resets accumulated price x volume and volume.
- Overnight sessions should be tested separately.

## Daily VWAP
- Daily VWAP resets on `timeframe.change("1D")`.
- No connection line should appear between completed and new daily periods when line-break mode is enabled.

## Weekly VWAP
- Weekly VWAP resets on `timeframe.change("1W")`.
- First trading day after the reset must begin a new weekly accumulation.

## Monthly VWAP
- Monthly VWAP resets on `timeframe.change("1M")`.
- First available trading bar of the new monthly period must begin a new monthly accumulation.

## Cross-asset checks
Test at minimum:
- XAUUSD / FX-style tick volume
- crypto
- exchange-traded equity or futures if available

## Timeframe checks
Test on:
- 5m
- 15m
- 1h
- 4h

Daily/weekly/monthly VWAPs become progressively less informative when chart timeframe approaches or exceeds their reset period; this is expected and should be documented rather than hidden.

## Acceptance gate
P02.0 is accepted only when all four VWAPs reset at the intended boundaries and no visual line-joining artefacts occur when reset-line breaking is enabled.
