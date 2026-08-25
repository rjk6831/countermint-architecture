# CounterMint system architecture

[← Back to the project overview](../README.md) · [Pricing decisions](decisions/) · [Engineering evidence](engineering-evidence.md)

CounterMint is a multi-tenant, headless appraisal, pricing, market-intelligence, and quoting platform. The architecture separates customer and dealer surfaces from one authoritative FastAPI application, keeps external market and AI providers behind controlled gateway layers, treats quote presentation as versioned domain state, and preserves customer-accepted price observations rather than recomputing or overwriting them.

## Platform topology

<p align="center">
  <a href="diagrams/countermint-topology.svg">
    <img src="diagrams/countermint-topology.svg" width="1200" alt="CounterMint platform topology and technical design">
  </a>
</p>

**[Open the full-resolution topology →](diagrams/countermint-topology.svg)**

### Current runtime boundaries

| Layer | Current implementation | Boundary |
|---|---|---|
| Public site | Separately hosted WordPress site | Marketing, acquisition, and checkout entry remain separate from application state |
| Dealer application | React 18 and Vite on Cloudflare Pages | Calls the headless API; no direct provider or database access |
| Customer quote | Public, tokenized quote surface | No customer account required; accepted observations append to history and later review creates a separate path |
| API | Python 3.12, async FastAPI, SQLAlchemy 2.0 | Owns tenancy, pricing, quoting, provenance, quote documents, billing, and provider coordination |
| Primary state | PostgreSQL 16 with Alembic | Authoritative transactional and pricing state |
| Media | Persistent local object-store volume behind a provider-neutral interface | Tenant-prefixed keys; an R2/S3 adapter is a future implementation, not a current claim |
| Edge and compute | Cloudflare, Caddy, Docker Compose, DigitalOcean | DNS and edge controls remain separate from origin TLS and application health checks |

## Architectural invariants

### 1. Models do not set prices

AI is useful for image segmentation, label extraction, and constrained quote-presentation assistance. Those workflows produce structured candidates that can be validated or rejected. Melt arithmetic, comp normalization, dealer bands, overrides, and final prices stay deterministic and auditable.

### 2. External pricing data has one gateway

Provider calls are metered and contracts change. A centralized service layer inside FastAPI owns caching, request deduplication, normalization, rate control, and cost observability. Internal consumers read canonical records rather than provider-shaped responses; this is an application boundary, not a separately deployed microservice.

### 3. Issuance, live drift, and acceptance have different authority

Issuance freezes the quote presentation and baseline spot set. While the quote remains open, eligible metal-bearing lines follow live spot drift. Customer acceptance appends the exact line-item value and spot tick to immutable history. Dealer confirmation can complete the quote or route upward drift through constrained review and a new counter path; neither path overwrites what the customer accepted.

### 4. Tenant context is required, not optional metadata

Workspace identity scopes authentication, staff permissions, settings, media keys, quote records, provider credentials, and data access. Cross-tenant behavior is tested as a security property rather than left to UI convention.

### 5. Provider-ready is not provider-live

The CDN identity, adapter, and BYOK boundary are implemented inside a separate CounterMint-only evaluation path. Production agent authorization, dealer BYOK activation, a platform-held feed, and any public CDN-derived display remain disabled pending written publisher authorization and a signed integration addendum. Recorded fixtures validate contracts but do not count as live-provider proof.

## Quote presentation as a domain model

Quote Studio does not store arbitrary markup. A versioned `QuoteDesignDocument` defines the approved presentation fields, references tenant-owned accepted assets, and uses optimistic concurrency for draft updates. The same renderer consumes explicit adapters for safe sample preview, tenant-private preview, customer output, and print mode.

Clicking a rendered logo, header, customer-action region, quote body, or footer selects the corresponding schema fields. Header artwork alone adds bounded pointer/keyboard positioning and locked-proportion resizing; this is not a freeform canvas.

Publication and issuance are separate authority changes:

1. draft edits create a new design revision;
2. explicit publishing activates a reviewed design;
3. quote issuance resolves the design, brand, and accepted assets into a presentation snapshot;
4. later design changes do not rewrite the issued quote;
5. customer/public asset access remains quote-token scoped.

Capability-gated AI work can propose text-free artwork or an approved style-token diff. It cannot modify pricing/customer data or publish. Durable job reservation, provider-start fencing, reconciliation, and settle-once accounting contain ambiguous billable outcomes.

## Quote lifecycle and pricing engine

<p align="center">
  <a href="diagrams/countermint-pricing-flow.svg">
    <img src="diagrams/countermint-pricing-flow.svg" width="1200" alt="CounterMint quote lifecycle, pricing engine, and market-data pipeline">
  </a>
</p>

**[Open the full-resolution pricing flow →](diagrams/countermint-pricing-flow.svg)**

### One eligible metal-bearing line item

1. Resolve metal, weight, fineness, date, mint, grade, holder, and provider identifiers.
2. Compute melt as deterministic arithmetic against the current normalized spot value.
3. Fetch sold comparables on demand, normalize them against historical spot, and reject mismatched or anomalous records.
4. Isolate the collector premium from the normalized market evidence.
5. Apply the dealer's versioned policy independently to each input class.
6. Persist source, timestamp, transformation, policy version, and resulting value.
7. Follow eligible spot drift while the quote remains open; append the customer's accepted observation, then confirm or create a separately traceable review/counter path.

Currency and other non-metal lines do not pretend to have melt or spot drift. Their evidence and policy paths remain explicit.

## Market sampling and native chart derivation

Metals Insights stores validated provider-delayed observations with provider and CounterMint receipt timestamps. Its research series remain separate from the established melt-valuation tables.

For each non-empty, UTC-aligned interval, CounterMint derives open from the first accepted sample, high and low from observed extrema, and close from the final sample. Sample count determines coverage. Missing intervals remain gaps; CounterMint does not interpolate observations it never received.

The dealer can switch instruments, line/candle presentation, ranges, and intervals and inspect accessible OHLC data. This is a CounterMint-native aggregation and rendering path over externally sourced observations—not an exchange feed and not automatic quote authority. [The market-intelligence note](market-intelligence.md) documents the full contract.

## State and failure containment

| Failure | Containment behavior |
|---|---|
| Spot provider unavailable | Serve explicitly timestamped cached state within policy; do not fabricate a live value |
| Sold comps sparse or noisy | Surface the evidence set and exclusions; retain deterministic dealer overrides |
| Model output incomplete | Require confirmation or fail the AI-assisted path; deterministic pricing remains separate |
| Greysheet credentials absent | Keep automated sheet pricing locked; manual values remain distinguishable from provider-derived values |
| Market moves after acceptance | Preserve the accepted observation; constrained review or countering creates a new traceable path rather than overwriting history |
| Provider response is ambiguous after a billable dispatch | Reconcile durable job state; do not blindly retry and risk duplicate spend |
| Deployment candidate drifts | Refuse release when the candidate does not match the tested commit receipt |

## Current, gated, and planned

| State | Components |
|---|---|
| **Operational private beta** | Web clients, FastAPI platform, PostgreSQL, local persistent media adapter, deterministic pricing, issued/accepted observations, provenance, roles, billing, vision intake, and customer/dealer counter workflows |
| **Implemented; environment release evidence required** | Quote Studio v3 structured editor and assets, terminal customer decline, broader Metals Insights workspace, locally sampled interactive charts, and staff attribution controls |
| **Capability/provider-gated** | AI-assisted Quote Studio artwork and bounded style proposals; publisher-authorization-gated CDN integration boundary |
| **Internal CounterMint evaluation only** | CounterMint-controlled CDN sandbox with provider-returned data confined to the protected surface and no public price-guide output |
| **Planned** | Evidence-backed technical context, richer chart interaction, AI Rarity Insights, native iOS/Android clients, and a cloud object-store adapter |

The design leaves room for later research, clients, and adapters without presenting them as shipped. See the explicit [roadmap and release gates](roadmap.md).
