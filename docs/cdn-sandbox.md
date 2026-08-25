# Internal CounterMint CDN integration sandbox

[← Back to the project overview](../README.md) · [Product tour](product-tour.md) · [System architecture](architecture.md)

CounterMint maintains a separate, access-controlled founder sandbox for CounterMint-only evaluation of a possible future Coin Dealer Newsletter (CDN) integration. It is ready to support a publisher review if CDN invites or authorizes one; it is not currently a joint evaluation, partnership, or production integration.

> **Commercial boundary:** CDN/Greysheet is a third-party publisher. This sandbox does not imply affiliation, endorsement, production authorization, redistribution rights, or general customer availability. Dealer BYOK activation and any public CDN-derived display remain disabled pending CDN's written authorization and a signed integration addendum. Final licensing interpretation belongs to CDN and counsel.

## The visible GSID control is the entry point

<p align="center">
  <a href="../screenshots/cdn-gsid-appraisal.png">
    <img src="../screenshots/cdn-gsid-appraisal.png" width="1100" alt="Standard CounterMint appraisal intake showing the GSID control that leads into the separate enhanced CDN sandbox workspace">
  </a>
</p>

<p align="center"><em>This is the standard New Appraisal screen, not the dedicated sandbox UI. GSID is the controlled entry point. In sandbox mode, resolving a supported GSID opens a separate enhanced dealer workspace with detailed item insights, protected reference context, independent cross-checks, versioned rule output, staging controls, and a contained DRAFT quote preview. That enhanced workspace is intentionally not pictured here.</em></p>

## What opens behind that control

| Capability | CounterMint behavior |
|---|---|
| Catalog identity | Keeps GSID, PCGS catalog number, Friedberg number, grading-service context, and holder certification serial distinct |
| Resolution priority | Resolves an exact catalog identity from the strongest available identifier rather than treating a slab serial as a pricing key |
| Grade context | Requires numeric grade and can select the relevant grade/CAC ladder context |
| Coin and currency coverage | Supports certified-coin and Friedberg paper-currency identity paths |
| Protected provider view | Presents authenticated reference and ladder information only inside the internal evaluation surface |
| Independent cross-checks | Keeps CounterMint melt, sold-comparable, and rule-based evidence separate from the provider reference |
| Derived offer | Applies the dealer's versioned range and recommendation logic to create a CounterMint-owned result |
| Quote staging | Lets the dealer add, inspect, edit, or remove staged items before generating a quote |
| Contained preview | Uses the actual customer renderer in a non-mutating DRAFT simulation without issuing, sharing, messaging, or persisting a public-link action |

## Why the enhanced sandbox UI is not pictured

CDN's published [API Terms of Use and License Agreement](https://www.greysheet.com/cms/1053/) treats GSID, catalog metadata, retail pricing, and wholesale pricing as different licensed-data classes. Public display rights differ by class, and wholesale values require prior written permission.

For that reason, this portfolio does not publish a real sandbox screenshot with only the prices blurred. A capture can still reveal licensed metadata, reference-ladder structure, branding, or other provider-returned context after the obvious numbers are covered. A future public walkthrough should use a fully synthetic demonstration record with no provider-returned data, or a specific capture set approved by CDN in writing. Until then, this page describes CounterMint's UI and workflow in text without presenting the normal appraisal screenshot as proof of the additional sandbox screens.

## End-to-end private workflow

1. **Enter exact identity.** The dealer supplies a numeric grade and one or more catalog identifiers. Metal, weight, and purity are optional for identity resolution but required for melt valuation.
2. **Resolve the catalog record.** CounterMint prefers explicit GSID, then supported catalog-number paths such as PCGS or Friedberg. Certification serial remains a separate holder identifier.
3. **Select the dealer-only reference context.** The sandbox chooses the matching grade and CAC ladder where available and records source, freshness, and confidence inside the protected workflow.
4. **Cross-check independently.** CounterMint calculates melt from its normalized spot path and keeps sold-comparable evidence independent of the provider reference.
5. **Apply dealer policy.** Versioned rules derive the suggested range and recommendation. Provider data does not bypass dealer controls.
6. **Stage before persistence.** `Add to quote` stages the item locally so details can be reviewed, corrected, or removed.
7. **Create a private draft.** Quote generation resolves identity, pricing, comps, spot, dealer rules, and recommendation into private DRAFT records.
8. **Preview the real customer UI safely.** A contained simulation renders the actual customer component from an allowlisted customer-safe projection without creating an issuable provider-derived quote.

## Isolation and retention controls

The sandbox is intentionally not a toggle on a normal production tenant.

- one founder-only dealer workspace;
- separate database and object storage;
- persistent `CDN SANDBOX — TEST DATA` identification;
- sandbox-specific routes and resource guards;
- operator scripts that reject production resources;
- a secretless tenant entitlement row and separately held deployment token;
- request-secret redaction from HTTP and exception logs;
- AES-256-GCM envelopes for dealer-supplied BYOK credentials if commercially authorized for activation;
- retention-bounded provider-returned raw observations;
- raw-value scrubbing before durable sandbox backup;
- exact-commit test receipt required before deployment;
- no public price guide, public customer link, or provider-returned wholesale payload in this repository.

## Customer-safe output contract

The internal dealer view may display provider-returned reference fields only within the account's permitted evaluation scope. No publisher, customer, or production use is claimed. The contained customer projection is deliberately narrower: it can show CounterMint-owned description, final offer, recommendation, and the normal quote presentation while excluding raw wholesale values, provider ladders, source metadata, and credentials.

This separation is structural. It is enforced at the API and renderer boundary rather than relying on a dealer to remember which fields are safe.

## Availability statement

| State | Meaning |
|---|---|
| **Implemented** | Identity resolver, protected provider adapter, dealer-only workflow, independent cross-checks, staging controls, derived offers, and contained DRAFT preview |
| **Internal CounterMint evaluation only** | CounterMint-controlled founder sandbox that is ready to support a publisher review only if CDN invites or authorizes one |
| **Disabled pending written agreement** | Production agent authorization, dealer BYOK activation, public/provider-derived display, and any platform-held feed |
| **Not claimed** | Partnership, endorsement, redistribution permission, general availability, or a public Greysheet product |

[Return to the product tour →](product-tour.md)
