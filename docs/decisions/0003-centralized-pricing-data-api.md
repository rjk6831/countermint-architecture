# ADR 0003: Centralize third-party pricing data behind one gateway layer

[← Decision index](README.md) · [Project overview](../../README.md)

**Status:** Accepted · **Date:** 2026-06 · **Last reviewed:** 2026-08-26

## Context

CounterMint depends on several external providers: spot prices for four metals, sold-comparable listings, and published sheet pricing. The quote service, valuation engine, appraisal pipeline, and background workers all need some part of that data.

The path of least resistance is for each service to call the provider it needs. It works immediately and it is what most codebases end up doing.

Three problems with it. Provider calls are metered and expensive, so N services calling the same endpoint means paying N times for one value. Provider contracts change, and when they do the fix is spread across every call site. And cost becomes unobservable — with calls scattered, nobody can answer what a given tenant costs in third-party data.

## Decision

Every provider call goes through one gateway layer inside the FastAPI application. Internal consumers do not talk to providers directly. This is a deliberate application boundary, not a separately deployed microservice.

That layer owns caching, request deduplication, rate control, normalization, and retention policy. One adapter exists per provider, so a contract change has one repair boundary. Internal consumers read canonical values rather than provider-shaped responses.

Spot refreshes through a PostgreSQL advisory-lock singleton on a sixty-second cycle. Sold comparables and CDN values are demand-driven and follow their own cache and retention rules. Versioned quote-design documents capture presentation state at issuance rather than silently refreshing an issued document in place.

## Consequences

**Good.** Repeated spot reads share one poller and canonical record instead of multiplying upstream calls. Provider spend and failures become observable at one boundary. Demand traffic and query breadth can still affect comparable-data spend, but caching and deduplication now have one accountable owner.

**Bad.** The gateway is a critical application boundary and needs to be correspondingly reliable. Cache invalidation and contract-bound data retention are real design problems with provider-specific tradeoffs. Indirection between a consumer and its upstream source adds debugging cost.

**Neutral.** This made the CDN/Greysheet client another adapter instead of a new integration pattern. A publisher-authorization-gated BYOK boundary and separate CounterMint-controlled evaluation sandbox are implemented, but production activation, a platform-held feed, and any public CDN-derived display remain disabled pending written permission and a signed integration addendum. Recorded fixtures validate the adapter contract but are not live-provider proof.

## What I would revisit

The gateway currently lives inside the main FastAPI deployment. Extracting it would add operational cost and should happen only if independent scaling or failure containment becomes more valuable than keeping the transaction and provider boundaries together.
