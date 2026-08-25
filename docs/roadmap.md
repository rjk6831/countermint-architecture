# CounterMint roadmap

[← Back to the project overview](../README.md) · [Quote Studio](quote-studio.md) · [Market intelligence](market-intelligence.md) · [System architecture](architecture.md)

This roadmap describes intended product direction, not released functionality or delivery dates. New work must preserve CounterMint's core rule: model output and market context can assist a dealer, but neither silently becomes pricing authority.

## Foundation already implemented

- explainable melt-versus-collector-premium valuation;
- sold-comparable normalization and reason-coded exclusions;
- versioned dealer pricing rules and Premium Protection;
- provider-delayed Metals Insights with locally sampled interactive line/candle charts;
- structured Quote Studio editing and immutable issued presentation snapshots;
- capability-gated AI artwork and bounded style proposals;
- customer accept, bounded counter, dealer counter-back, rejection, and terminal per-item decline;
- tenant-scoped staff roles, quote attribution, provenance, and audit history;
- private, authorization-gated CDN integration sandbox.

## Near term

### 1. Explainable technical-analysis context

Add basic trend and momentum context over completed locally sampled candles.

Required product contract:

- versioned indicator and parameter definitions;
- traceable source intervals and calculation timestamps;
- minimum sample and coverage gates;
- completed-candle behavior;
- freshness requirements and expiry;
- inspectable reasons for each signal;
- decision-support-only language;
- no automatic mutation of a quote, suggested range, or final offer.

These signals are market context for a dealer, not executable trading advice.

### 2. Richer market exploration

Build on the existing interactive line/candle, range, interval, and instrument controls with deeper comparison, inspection, and overlay tools. Gaps, provider delay, local sampling, and partial coverage must stay visible as interaction becomes richer.

### 3. AI Rarity Insights

Add a tenant-scoped research workflow for requesting an in-depth review of one coin from a configured foundation model.

Planned controls:

- dealer-initiated requests only;
- per-dealer query allowances;
- allowance and provider-budget scaling by subscription tier;
- rate, concurrency, and cost ceilings;
- bounded input and output contracts;
- retained request status and audit attribution;
- guardrails against presenting model output as authentication, grading, provenance, rarity certification, or pricing authority;
- no automatic quote mutation.

The intended value is faster research triage and a more useful dealer briefing—not an AI-generated grade or price.

## Later platform work

### Native mobile clients

Add iOS and Android capture/review experiences over the existing headless API rather than creating a second valuation engine.

### Cloud object-storage adapter

Add an R2/S3-compatible implementation behind the existing provider-neutral media interface while preserving tenant-prefixed keys, derivative validation, and immutable issued-asset behavior.

### First-class premium confidence

Expose calibrated premium-confidence inputs and evidence quality as dealer-facing controls without hiding sparse comparables behind a false precision score.

## Release gates for roadmap capabilities

| Area | Minimum gate before a shipped claim |
|---|---|
| Technical context | Deterministic calculations, historical fixtures, coverage/freshness behavior, expiry, accessible explanation, and separation from pricing |
| AI Rarity Insights | Tenant isolation, allowance enforcement, budget ceilings, idempotency, provider ambiguity reconciliation, privacy review, and explicit non-authority language |
| Richer charts | Browser evidence across ranges and partial data, performance bounds, accessibility, and preserved gap disclosure |
| Mobile | Auth, tenant boundary, API parity, offline/error behavior, and release-store evidence |
| Cloud media | Tenant-fenced keys, safe derivatives, retention, issued-snapshot compatibility, and migration/readback proof |

## What will remain invariant

1. Models do not set prices.
2. Customer-accepted observations are never overwritten.
3. Provider data remains subject to contract, attribution, retention, and display boundaries.
4. Research surfaces do not silently alter the authoritative quote path.
5. Subscription metering cannot weaken tenant isolation or auditability.
6. “Implemented,” “enabled,” “deployed,” and “verified live” remain separate evidence states.
