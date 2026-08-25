# ADR 0001: Separate melt value from numismatic premium

[← Decision index](README.md) · [Project overview](../../README.md)

**Status:** Accepted · **Date:** 2026-05 · **Last reviewed:** 2026-08-26

## Context

Sold-comparable data reports one number: what somebody paid. That number is melt value plus collector premium plus noise, fused together.

A dealer's economics treat those components completely differently. Melt is metal risk — it moves with spot and can be hedged with standard metal instruments. Premium is collector risk — it moves slowly with the numismatic market and is not directly hedgeable through those instruments. A dealer's tolerance for the two is not the same, and it is not the same on the buy side as the sell side.

Quoting off raw comparables is the simple implementation. It is also the reason a dealer cannot express their actual policy: any control they set applies to a blended number, so tightening exposure to metal necessarily tightens exposure to collector value too, and vice versa.

## Decision

Compute the two on separate tracks and combine them only after policy is applied.

Melt is deterministic arithmetic: weight × fineness × current spot for the relevant metal. Premium is derived: reconcile units, reject mismatches and outliers, compute each comparable's melt against spot at its own sale date, then subtract that historical melt from its sold value to isolate collector premium.

Dealers then configure bands and biases independently against each input: spot, numismatic premium, and CDN/Greysheet sheet price. Buy-side and sell-side policy differ, and both belong to the dealer rather than to us.

## Consequences

**Good.** A dealer can hold metal exposure tight while staying loose on collector value, or the reverse, which is what they actually want to do. Eligible metal-bearing lines can follow current spot without rewriting the premium evidence. When a price looks wrong, the responsible component is identifiable rather than buried in a blend.

**Bad.** Meaningfully more work than quoting off comps. Normalization is the hard part: comparables carry inconsistent units, grades, and holder descriptions, and outlier rejection is a judgment encoded in code. Premium quality depends entirely on comparable coverage, which is thin for scarce items.

**Neutral.** The separation is what makes CDN/Greysheet integration additive rather than disruptive — sheet pricing becomes a third configurable input rather than a replacement for the model.

## What I would revisit

Outlier rejection is currently rule-based. With enough volume, a distributional approach would be better, and the current rules would look crude in hindsight.

Comparable counts, match scores, and inclusion decisions are visible. What is still missing is a calibrated aggregate confidence signal that distinguishes broad, consistent evidence from a sparse or unstable comparable set without hiding the underlying records.
