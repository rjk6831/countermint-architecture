# Metals Insights and native market-series derivation

[← Back to the project overview](../README.md) · [System architecture](architecture.md) · [Pricing decisions](decisions/) · [Roadmap](roadmap.md)

Metals Insights is CounterMint's dealer-facing market workspace. It covers the four core quote metals—gold, silver, platinum, and palladium—plus 16 additional metal instruments. It starts with externally sourced observations delayed by one minute, then validates, stores, aggregates, and renders the history CounterMint actually received.

“Native” refers to CounterMint's sampling, persistence, interval derivation, API, interactive line/candlestick charting, and disclosure logic. It does not mean CounterMint originates exchange prices or provides a live or executable trading feed.

## Two market surfaces, two responsibilities

| Surface | Purpose | Authority |
|---|---|---|
| Core metal snapshot | Keep gold, silver, platinum, and palladium visible throughout appraisal and quote work | Timestamped reference context for the established melt-pricing path |
| Metals Insights | Explore 20 instruments through local history, interactive line/candlestick views, ranges, intervals, and OHLC tables | One-minute-delayed dealer research context; never quote or trading authority |

Keeping these responsibilities separate prevents an exploratory chart feature from becoming an accidental pricing engine.

## Collection and normalization

One enriched provider request supplies the curated market catalog and core bid/ask symbols under the configured plan. CounterMint validates the provider response before accepting it:

- provider success and expected USD base;
- bounded numeric values and timestamps;
- direct-USD preference where available;
- reciprocal conversion with bid/ask-side correction when necessary;
- duplicate and out-of-order protection;
- provider and CounterMint receipt timestamps;
- explicit freshness and stale-state disclosure.

The broader catalog is stored independently from the legacy valuation tables so new research symbols cannot change melt calculations by appearing in the Insights workspace.

## How an indicative candle is derived

CounterMint groups stored observations into validated, UTC-aligned intervals. For each non-empty interval:

| Field | Derivation |
|---|---|
| **Open** | First accepted observation in the interval |
| **High** | Maximum accepted observation in the interval |
| **Low** | Minimum accepted observation in the interval |
| **Close** | Final accepted observation in the interval |
| **Samples** | Number of accepted observations contributing to the interval |
| **Coverage** | Observed sample count compared with the expected sampling opportunity |

No synthetic ticks are inserted. If CounterMint did not receive a usable observation for an interval, the API omits that bucket and the chart preserves the gap.

These are therefore **locally sampled, indicative OHLC candles**, not exchange candles. The distinction is visible in the product rather than hidden in a footnote.

## Current dealer experience

The implemented workspace supports:

- a concise four-metal header across dealer workflows;
- a searchable and filterable catalog of 20 instruments: four core quote metals and 16 additional metals;
- interactive native line and candlestick presentation;
- validated range and interval controls;
- zoom, pan, cursor inspection, bucket pinning, and previous/next navigation;
- visible source, provider sample time, age/freshness, sample count, and coverage;
- accessible OHLC tabular output alongside the chart;
- explicit empty, partial-coverage, delayed, and stale states;
- prior-sample movement for green/red direction instead of mislabeling bid/ask spread as price movement.

## Failure behavior

| Condition | Product response |
|---|---|
| Provider temporarily unavailable | Preserve timestamped cached state within policy; never label it live |
| Partial interval coverage | Show the sample count and coverage instead of implying completeness |
| Missing interval | Preserve a visible gap; do not interpolate |
| Duplicate or older observation | Reject it from the canonical local sequence |
| Unsupported symbol or interval | Reject the request through the validated API contract |
| Research data incomplete | Keep deterministic quote pricing on its separate authoritative path |

## Roadmap: technical context, not trading authority

The next planned layer is evidence-backed technical-analysis context over completed local candles. Any signal contract must include:

- versioned indicator and parameter definitions;
- traceable input intervals and calculation timestamps;
- minimum sample and coverage requirements;
- completed-candle rules to avoid unstable partial-period signals;
- freshness gates and explicit expiry;
- an inspectable explanation of why a signal appeared;
- decision-support-only language;
- strict separation from automatic offer calculation.

The existing charts are already interactive. Roadmap work will deepen comparison, inspection, and overlays rather than presenting basic chart interactivity as unfinished.

[Review the broader roadmap →](roadmap.md)
