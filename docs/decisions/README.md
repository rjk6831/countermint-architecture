# Architecture decision records

[← Back to the project overview](../../README.md) · [System architecture](../architecture.md)

These records capture the decisions that most directly affect pricing correctness, operating cost, and auditability. Each ADR includes the downside and what should be revisited, because the tradeoff is part of the decision.

| ADR | Decision | Failure it prevents | Cost accepted |
|---|---|---|---|
| [0001](0001-melt-vs-numismatic-premium.md) | Separate melt value from collector premium | One blended price hides whether metal or collector risk moved | More normalization work and visible dependence on comparable coverage |
| [0002](0002-immutable-quote-snapshots.md) | Preserve issued and customer-accepted observations through review and confirmation | A customer-accepted value is overwritten during an asynchronous handshake | More storage and distinct issued, accepted, reviewed, and confirmed paths |
| [0003](0003-centralized-pricing-data-api.md) | Centralize provider data behind one gateway layer | Duplicate metered calls, scattered contract fixes, and unobservable spend | A critical application boundary and explicit cache-staleness decisions |
| [0004](0004-value-provenance.md) | Attach source and rule lineage to every priced component | A challenged or incorrect number cannot be reproduced later | Higher write volume and provenance context throughout the engine |

## Reading order

Start with [ADR 0001](0001-melt-vs-numismatic-premium.md) for the domain model, then [ADR 0002](0002-immutable-quote-snapshots.md) for temporal correctness. [ADR 0003](0003-centralized-pricing-data-api.md) explains the provider boundary, and [ADR 0004](0004-value-provenance.md) completes the auditability model.
