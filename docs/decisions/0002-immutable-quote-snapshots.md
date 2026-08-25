# ADR 0002: Preserve issued and customer-accepted value observations

[← Decision index](README.md) · [Project overview](../../README.md)

**Status:** Accepted · **Date:** 2026-05 · **Last reviewed:** 2026-08-26

## Context

A quote is a commitment made against a market that keeps moving. Spot changes while the quote sits in a customer's inbox.

The natural implementation computes price on read. It gives the customer a live number while the quote is open, which is exactly right — and it gives the wrong answer the moment they accept. If acceptance is recorded but price is still computed on read, then a quote accepted on Tuesday shows Wednesday's number on Wednesday.

That failure is quiet. Nothing errors. The dealer's position changes silently and nobody notices until reconciliation, which in this business may be weeks later and may be never.

## Decision

Issuance freezes the quote presentation and baseline spot set. While the quote remains open, eligible metal-bearing lines can follow live spot drift. Customer acceptance appends the exact line-item values and spot tick offered at that moment to immutable history.

Dealer confirmation can complete the quote or route upward drift through constrained review and a new counter path. This separates four authorities: the issued baseline, the live open-quote view, the customer-accepted observation, and any later reviewed or confirmed result. Currency and other non-metal lines do not pretend to have spot drift. No later transition can rewrite what the customer actually accepted.

A dedicated pricing-drift regression suite exists specifically to catch violations of this, because it is the kind of bug that reintroduces itself during unrelated refactors and is invisible in manual testing.

## Consequences

**Good.** The customer-accepted observation is stable even when the dealer must review upward drift. Accepted values are stored rather than re-derived, so later review begins from the actual offer and evidence instead of today's market.

**Bad.** Storage grows with issued, accepted, reviewed, and confirmed observations because history is append-only rather than overwritten. Multiple pricing and state paths must stay aligned, which is precisely what the regression suite guards.

**Neutral.** Counter-offers complicate the state machine. A dealer-authored counter-offer is fixed deliberately rather than immediately re-drifted, while other open metal-bearing lines may still follow spot. Immutability therefore belongs to recorded observations, not to the quote object in every state.

## What I would revisit

Observations currently store inputs inline. At volume, content-addressed storage for common inputs would cut duplication substantially without weakening the history guarantee.

The accepted-but-awaiting-confirmation state deserves continuing operational scrutiny: retries and recovery must preserve the customer's captured values while making any review or counter path unmistakably separate.
