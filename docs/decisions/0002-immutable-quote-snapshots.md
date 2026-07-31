# ADR 0002: An accepted quote freezes to an immutable snapshot

**Status:** Accepted · **Date:** 2026-05

## Context

A quote is a commitment made against a market that keeps moving. Spot changes while the quote sits in a customer's inbox.

The natural implementation computes price on read. It gives the customer a live number while the quote is open, which is exactly right — and it gives the wrong answer the moment they accept. If acceptance is recorded but price is still computed on read, then a quote accepted on Tuesday shows Wednesday's number on Wednesday.

That failure is quiet. Nothing errors. The dealer's position changes silently and nobody notices until reconciliation, which in this business may be weeks later and may be never.

## Decision

Acceptance freezes the full priced snapshot: every input, every rule applied, and every resulting number, captured at the moment of agreement.

While a quote is open it reprices continuously against live spot, because that is genuinely what the customer should see. On acceptance the computation stops and the stored result becomes authoritative. Subsequent market movement cannot alter an agreed price.

A dedicated pricing-drift regression suite exists specifically to catch violations of this, because it is the kind of bug that reintroduces itself during unrelated refactors and is invisible in manual testing.

## Consequences

**Good.** An agreed price is an agreed price. The dealer's position is stable once the handshake happens. Every accepted quote is fully auditable after the fact, because the inputs are stored rather than re-derived — a dispute is answerable months later.

**Bad.** Storage grows with accepted quotes, since snapshots hold the full input set rather than references. Two code paths now exist for pricing — live and frozen — and they must not diverge, which is precisely what the regression suite guards.

**Neutral.** Counter-offers complicate the state machine. A countered quote returns to live pricing until it is accepted or expires, so "frozen" is a property of acceptance rather than of the quote object.

## What I would revisit

Snapshots currently store inputs inline. At volume, content-addressed storage for the common inputs would cut duplication substantially without weakening the guarantee.

There is also no expiry on open quotes. A quote left open indefinitely reprices indefinitely, which is defensible but probably not what a dealer intends.
