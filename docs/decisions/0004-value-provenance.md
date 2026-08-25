# ADR 0004: Every quoted number carries its source and rule

[← Decision index](README.md) · [Project overview](../../README.md)

**Status:** Accepted · **Date:** 2026-07 · **Last reviewed:** 2026-08-26

## Context

A dealer looks at a quoted number and asks where it came from. Sometimes because they are checking our work, sometimes because a customer is challenging the price, sometimes because the number is simply wrong and needs fixing.

Without provenance, answering that means re-deriving the value from current data — which no longer reproduces the original, because spot has moved and comparables have changed. The honest answer becomes "I am not sure," and that answer is fatal for a pricing product. A dealer who cannot defend a number to a customer will not use the tool.

The same gap makes debugging archaeology. A wrong price could originate in a bad comparable, a misparsed sheet record, a misapplied band, or an identification error, and distinguishing them after the fact requires reconstructing state that is gone.

## Decision

Every component of a quoted value records the source it came from and the rule that produced it at computation time, subject to provider-specific retention limits.

Melt records which spot value and which timestamp. Premium records which comparables contributed and what normalization was applied. Sheet pricing records the resolved GSID and the specific record matched. Each dealer band or bias records which policy was applied and what it did to the number.

GSID resolution is part of this: matching an item to a published sheet record is itself a decision that can be wrong, so the resolution is stored rather than recomputed.

## Consequences

**Good.** A challenged price begins as a lookup rather than an argument. Debugging a wrong quote points at a component instead of requiring reconstruction. Combined with frozen snapshots, accepted values retain safe lineage; where licensing requires raw-value deletion, attribution and non-redistributable evidence remain without pretending the purged payload is still present.

**Bad.** Provenance records are written on every valuation, which is a meaningful volume of writes for data that is mostly never read. Every pricing component now has to thread a provenance context, which is friction on every change to the engine.

**Neutral.** Provenance turned out to be the fastest way to find bugs during development, which was not the reason it was built.

## What I would revisit

Retention is provider-specific, especially for provider-returned wholesale values. The next step is a more strongly typed cross-provider provenance envelope so retention-safe evidence has the same shape without erasing important provider differences.

The dealer-facing view now exposes comparable evidence, match scores, and include/exclude decisions. It should evolve into an exportable dispute record and add a calibrated aggregate confidence summary without hiding the underlying evidence.
