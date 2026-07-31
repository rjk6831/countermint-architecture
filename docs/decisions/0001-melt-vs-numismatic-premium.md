# ADR 0001: Separate melt value from numismatic premium

**Status:** Accepted · **Date:** 2026-05

## Context

Sold-comparable data reports one number: what somebody paid. That number is melt value plus collector premium plus noise, fused together.

A dealer's economics treat those components completely differently. Melt is metal risk — it moves minute to minute with spot and is hedgeable. Premium is collector risk — it moves slowly with the numismatic market and is not hedgeable at all. A dealer's tolerance for the two is not the same, and it is not the same on the buy side as the sell side.

Quoting off raw comparables is the simple implementation. It is also the reason a dealer cannot express their actual policy: any control they set applies to a blended number, so tightening exposure to metal necessarily tightens exposure to collector value too, and vice versa.

## Decision

Compute the two on separate tracks and combine them only after policy is applied.

Melt is deterministic arithmetic: weight × fineness × current spot for the relevant metal. Premium is derived — normalize sold comparables, reject outliers, reconcile units, then subtract computed melt to isolate a per-item collector premium.

Dealers then configure bands and biases independently against each input: spot, numismatic premium, and CDN/Greysheet sheet price. Buy-side and sell-side policy differ, and both belong to the dealer rather than to us.

## Consequences

**Good.** A dealer can hold metal exposure tight while staying loose on collector value, or the reverse, which is what they actually want to do. Melt updates continuously without disturbing premium. When a price looks wrong, the component responsible is identifiable rather than buried in a blend.

**Bad.** Meaningfully more work than quoting off comps. Normalization is the hard part: comparables carry inconsistent units, grades, and holder descriptions, and outlier rejection is a judgment encoded in code. Premium quality depends entirely on comparable coverage, which is thin for scarce items.

**Neutral.** The separation is what makes CDN/Greysheet integration additive rather than disruptive — sheet pricing becomes a third configurable input rather than a replacement for the model.

## What I would revisit

Outlier rejection is currently rule-based. With enough volume, a distributional approach would be better, and the current rules would look crude in hindsight.

Premium confidence is also not surfaced. A dealer sees a premium derived from forty comparables and one derived from two, presented identically. That is a real gap and is not yet built.
