# CounterMint™

**A market-aware appraisal, pricing, and quote-experience platform for coin, currency, and precious-metals dealers.**

CounterMint takes a dealer from intake to a defensible valuation, a branded customer quote, and a governed accept/counter/decline workflow. It combines volatile metal markets, sold-comparable evidence, dealer policy, visual quote design, staff authority, and customer response history without turning any one provider—or an AI model—into an unaccountable price oracle.

**Private beta · Designed, built, deployed, and operated by Raymond J. Kraft**

[View a live customer quote](https://app.countermint.io/q/5eiJMiqsuZgLrZdtKfYuze3W8RrgL6qYuw4bIcIbkvw) · [Request a walkthrough](mailto:ray@rootsnolimits.com)

| Automated verification | Measured project footprint | Schema evolution | Core quote metals |
|---|---|---|---|
| **700+ backend and frontend tests** | **92.5K+ lines** across 429 source, test, configuration, and documentation files | **40+ Alembic migrations** | Gold, silver, platinum, and palladium |

*Private application repository snapshot measured August 24, 2026. See the [measurement basis and evidence boundaries](docs/engineering-evidence.md).*

**Explore the work:** [Product tour](docs/product-tour.md) · [Quote Studio](docs/quote-studio.md) · [Market intelligence](docs/market-intelligence.md) · [System architecture](docs/architecture.md) · [Decision records](docs/decisions/) · [Roadmap](docs/roadmap.md) · [Engineering evidence](docs/engineering-evidence.md)

## Latest product build — August 2026

### Quote Studio v3: one renderer from design to customer response

<p align="center">
  <a href="screenshots/quote-studio-overview.png">
    <img src="screenshots/quote-studio-overview.png" width="1100" alt="CounterMint Quote Studio showing a structured quote preview, design library, desktop mobile and print modes, clickable sections, and gated AI-assisted design controls">
  </a>
</p>

<p align="center"><em>Click a part of the quote, edit its approved fields, preview desktop, mobile, or print, and publish a versioned design. The AI lane was switched off in this environment; the full manual editor still works.</em></p>

<p align="center">
  <a href="screenshots/quote-studio-header-artwork.png">
    <img src="screenshots/quote-studio-header-artwork.png" width="1100" alt="Quote Studio header artwork controls showing a customer-safe quote preview, frame, fit, focal position, zoom, edge treatment, and optimized derivative workflow">
  </a>
</p>

<p align="center"><em>This is the actual quote renderer, not a mockup. Clicking the header opens the fields bound to that section of the design JSON.</em></p>

<p align="center">
  <a href="screenshots/quote-studio-image-controls.png">
    <img src="screenshots/quote-studio-image-controls.png" width="420" alt="Expanded Quote Studio image controls for framing, fit, focal position, zoom, brightness, contrast, saturation, overlay, and optimized derivative review">
  </a>
</p>

<p align="center"><em>Dealers can frame, crop, position, zoom, and tune artwork without opening up arbitrary CSS or a free-form canvas.</em></p>

<p align="center">
  <a href="screenshots/quote-studio-customer-actions.png">
    <img src="screenshots/quote-studio-customer-actions.png" width="1100" alt="Quote Studio previewing accept, counter-offer, and terminal item-decline actions with dealer-configurable presentation labels">
  </a>
</p>

<p align="center"><em>Dealers can change the button labels and choose whether item decline appears. CounterMint still owns the state machine behind accept, counter, counter-back, reject, and terminal decline.</em></p>

### Dealer controls, staff authority, and protected evaluation

<p align="center">
  <a href="screenshots/pricing-rules-default.png">
    <img src="screenshots/pricing-rules-default.png" width="1100" alt="CounterMint versioned pricing rules showing the default offer range and Premium Protection tiers">
  </a>
</p>

<p align="center"><em>Pricing Rules version 2 is active here. Saving a change creates a new version, so an older quote keeps the policy that priced it. Premium Protection adjusts the collector-premium side without reducing current melt.</em></p>

<p align="center">
  <a href="screenshots/staff-roles-attribution-redacted.png">
    <img src="screenshots/staff-roles-attribution-redacted.png" width="1100" alt="CounterMint Staff and Roles workspace with public contact details redacted, showing role, permissions, activity, status, and lifecycle controls">
  </a>
</p>

<p align="center"><em>Roles, pricing permissions, status, and staff activity live in one place. Email addresses and phone numbers are blacked out in this public image.</em></p>

<p align="center">
  <a href="screenshots/cdn-gsid-appraisal.png">
    <img src="screenshots/cdn-gsid-appraisal.png" width="1100" alt="Standard CounterMint appraisal intake showing the GSID control that leads into a separate enhanced CDN sandbox workspace">
  </a>
</p>

<p align="center"><em>This is the normal appraisal screen, not the CDN sandbox UI. GSID is the entry point: in sandbox mode it opens a separate enhanced dealer workspace for detailed item insights, protected reference context, independent cross-checks, versioned rules, and DRAFT quote review. That additional UI is intentionally not shown publicly.</em></p>

---

## More than a pricing calculator

| Product surface | Dealer experience | Engineering boundary |
|---|---|---|
| **Appraisal intake** | Manual entry, multi-item image capture, label extraction, and distinct catalog identifiers | Uncertain fields are staged for confirmation instead of guessed |
| **Explainable valuation** | Melt, collector premium, sold comps, exclusions, source, and policy are inspectable separately | Deterministic arithmetic and rule execution remain authoritative |
| **Metals Insights** | Searchable market catalog with interactive line/candle views, ranges, intervals, freshness, and OHLC tables | Provider-delayed observations; CounterMint-native aggregation with no invented buckets |
| **Pricing policy** | Versioned offer ranges, transaction-side controls, and Premium Protection | Historical quotes continue to resolve against the policy that priced them |
| **Quote Studio** | Presets, click-to-edit regions, brand controls, artwork composition, customer actions, and responsive preview | Strict versioned design JSON and one shared customer/preview/print renderer |
| **Customer negotiation** | Accept, customer counter, dealer counter-back, rejection, and terminal per-item decline | State-checked server lifecycle despite dealer-configurable labels and thresholds |
| **Staff governance** | Invitations, roles, status, pricing authority, and dealer-approved quote attribution | Tenant-scoped authorization and append-only audit history |
| **Internal CDN evaluation** | Catalog identity resolution and a protected reference workflow in an isolated CounterMint sandbox | No public guide, public provider data, or production activation pending written authorization |

## CounterMint in 90 seconds

1. **Read the market.** The workspace keeps the four core quote metals visible while Metals Insights opens a wider searchable catalog and the locally sampled history CounterMint has actually received.
2. **Capture the item.** A dealer enters it manually or photographs multiple coins and slabs. Vision assists segmentation and label extraction; uncertain fields require confirmation.
3. **Resolve the value.** Deterministic arithmetic computes melt. Sold comparables are normalized against historical spot to isolate collector premium, with inclusion and exclusion evidence preserved.
4. **Apply dealer policy.** Versioned ranges and Premium Protection operate on explicit inputs instead of hiding the dealer's strategy inside one opaque adjustment.
5. **Design the quote.** Quote Studio binds branding, typography, artwork, sections, and customer actions to a validated design document rendered against safe sample data or a real customer-safe quote.
6. **Negotiate remotely.** The customer can accept, submit a bounded counter, respond to a dealer counter, or decline an item. The dealer can configure limits, thresholds, presentation labels, and terminal decline availability.
7. **Preserve the handshake.** Acceptance appends the exact value and spot observation. Confirmation, review, or countering creates a traceable next state without overwriting what the customer accepted.

---

## Quote Studio: the quote experience is structured product state

Quote Studio is not a free canvas, a screenshot generator, or arbitrary CSS pasted onto customer data. It is a schema-backed editor around a versioned `QuoteDesignDocument` and a shared renderer.

- **Click the rendered quote.** Logo, header, customer actions, body, and footer regions open the controls bound to that section's validated JSON fields.
- **Manipulate artwork without breaking layout.** Header artwork supports bounded pointer and keyboard positioning, locked-proportion resizing, pan, zoom, fit, edge treatment, brightness, contrast, saturation, and overlay controls.
- **Start curated, then personalize.** Dealers can create from approved presets, duplicate designs, edit drafts, undo/redo, and preview desktop, mobile, and print before publishing.
- **Keep media private until accepted.** Uploaded or generated artwork begins as a private proposal. CounterMint validates the raster, strips unsafe metadata into optimized derivatives, and requires review before it can enter a draft or published design.
- **Use AI as a proposal engine.** Capability-gated pipelines can produce private text-free header artwork, symbol-first logo concepts, and bounded style proposals. AI cannot change quote values, customer data, lifecycle state, or publishing state; results remain inert until the dealer explicitly applies them.
- **Freeze what the customer received.** Publishing activates a versioned design, while quote issuance resolves the exact presentation and assets into its immutable presentation snapshot.

[Explore the Quote Studio design and safety model →](docs/quote-studio.md)

---

## Metals Insights: external observations, native derivation

CounterMint does not claim to originate exchange data. It ingests provider-delayed observations, validates and timestamps what arrived, and then stores, aggregates, and renders its own market series in PostgreSQL.

For every completed interval:

- the first received observation becomes **open**;
- observed extrema become **high** and **low**;
- the final observation becomes **close**;
- sample count and expected coverage stay visible;
- empty buckets remain gaps rather than being interpolated into false history.

Dealers can already switch instruments, line/candle presentation, date ranges, and sampling intervals, with accessible OHLC tables and freshness disclosures. The broader research surface remains separate from the authoritative melt path so exploratory charts cannot silently rewrite a quote.

[See how CounterMint derives and governs market series →](docs/market-intelligence.md)

---

## Pricing policy that remains explainable later

<p align="center">
  <a href="screenshots/quote-melt-premium-breakdown.png">
    <img src="screenshots/quote-melt-premium-breakdown.png" width="1000" alt="Earlier private-beta CounterMint UI showing separate melt value, collector premium, current comparable value, versioned dealer-policy range, and final offer">
  </a>
</p>

<p align="center"><em><strong>Earlier private-beta UI.</strong> The layout has changed, but the contract has not: melt, collector premium, market evidence, the versioned policy range, and the final offer remain separate.</em></p>

---

## Customer negotiation, end to end

Customer negotiation is implemented as a state machine, not a collection of cosmetic buttons:

- dealer opt-in and per-item customer attempt limits;
- manual review or direction-aware automated thresholds;
- dealer accept, reject, or counter-back;
- customer acceptance or rejection of a dealer counter;
- a separate, idempotent terminal decline for one item while other quote lines remain available;
- audit events and immutable history across every transition.

Quote Studio lets the dealer choose approved labels and whether terminal decline appears, but presentation never redefines the server-owned lifecycle meaning.

---

## Private CDN integration sandbox

CounterMint maintains a separate, access-controlled founder sandbox for CounterMint-only evaluation of a possible future Coin Dealer Newsletter integration. It is ready to support a publisher review if CDN invites or authorizes one; it is not currently a joint evaluation, partnership, or production integration. The sandbox demonstrates catalog identity across GSID, PCGS catalog, and Friedberg identifiers; grading-service and CAC context; protected reference ladders; independent melt and comparable-sales cross-checks; versioned dealer rules; and a contained customer-safe DRAFT preview.

The GSID image near the top of this README shows only the normal appraisal entry point. It is not a capture of—and should not be read as representative of—the separate enhanced sandbox interface described here.

Provider-returned wholesale values remain authenticated and confined to the protected evaluation surface. Raw observations are retention-bounded, request secrets are log-redacted, and the sandbox cannot issue or share a public provider-derived quote. Dealer BYOK activation and any public CDN-derived display remain disabled pending CDN's written authorization and a signed integration addendum.

**CDN/Greysheet is a third-party publisher. No affiliation, endorsement, redistribution right, or production availability is implied.**

[Review the demonstrated workflow and licensing boundary →](docs/cdn-sandbox.md)

---

## Delivery status

| Status | Scope |
|---|---|
| **Operational private beta** | Appraisal intake, deterministic pricing, sold-comp provenance, versioned dealer rules, live customer quotes, customer/dealer counter workflows, staff roles, billing, and accepted-value history |
| **Implemented; exact release evidence remains environment-specific** | Quote Studio v3 structured editor and assets, terminal customer decline, broader Metals Insights workspace, interactive locally sampled charts, and new staff attribution controls |
| **Implemented but capability/provider-gated** | AI-assisted Quote Studio artwork and bounded style proposals; publisher-authorization-gated CDN integration boundary |
| **Internal CounterMint evaluation only** | CounterMint-controlled CDN sandbox; no general customer availability or public provider-data display |
| **Roadmap, not shipped** | Evidence-backed technical-analysis context, richer chart interaction, AI Rarity Insights, native mobile clients, and a cloud object-store adapter |

The product code is private. This repository publishes architecture, operating boundaries, product evidence, and consequential design decisions without exposing proprietary source or customer secrets.

---

## Architecture at a glance

<p align="center">
  <a href="docs/diagrams/countermint-topology.svg">
    <img src="docs/diagrams/countermint-topology.svg" width="1000" alt="CounterMint platform topology showing client surfaces, FastAPI services, PostgreSQL, tenant-scoped object storage, and external providers">
  </a>
</p>

<p align="center"><em>A headless FastAPI application serves the dealer SPA and customer quote surface. PostgreSQL is authoritative; external market data enters through one normalized provider gateway.</em></p>

**[Open the full-resolution topology →](docs/diagrams/countermint-topology.svg)**

Five boundaries do most of the risk-reduction work:

1. **Pricing stays deterministic.** Models assist extraction, research, and presentation work; they do not compute melt, collector premium, policy bands, or final price.
2. **Providers have one door.** Caching, request deduplication, normalization, rate control, and spend observability live behind one internal data gateway.
3. **Open and accepted values have different authority.** Open metal-bearing lines can follow the market; customer-accepted observations are append-only even when review creates a new counter path.
4. **Every value carries retention-safe lineage.** Source attribution, timestamps, normalization decisions, and dealer rules are retained subject to provider policy.
5. **Tenant context is structural.** Authentication, roles, media keys, settings, AI budgets, and data access are scoped to the dealer workspace.

The detailed [architecture guide](docs/architecture.md) separates current, release-gated, provider-gated, private-evaluation, and planned components.

## Four decisions that define the system

| Constraint | Decision | Deliberate cost | Record |
|---|---|---|---|
| Spot risk and collector risk behave differently | Compute melt and collector premium separately, then apply policy to each | More normalization work; sparse comps remain a real limitation | [ADR 0001](docs/decisions/0001-melt-vs-numismatic-premium.md) |
| A live market must not erase what the customer accepted | Freeze presentation and baseline spot on issuance, then append the accepted observation during the customer/dealer handshake | More storage and distinct open, accepted, reviewed, and confirmed paths to keep aligned | [ADR 0002](docs/decisions/0002-immutable-quote-snapshots.md) |
| Metered provider calls become expensive and inconsistent when scattered | Put every external pricing source behind one internal gateway | A new reliability boundary and explicit cache-staleness tradeoffs | [ADR 0003](docs/decisions/0003-centralized-pricing-data-api.md) |
| A dealer must be able to defend a number later | Persist retention-safe source and rule lineage for every pricing component | Higher write volume, provider-specific purges, and provenance threaded through the engine | [ADR 0004](docs/decisions/0004-value-provenance.md) |

## Engineering evidence

| Risk | Control |
|---|---|
| Silent price drift | Dedicated regression coverage for issued, accepted, reviewed, and confirmed value boundaries |
| Cross-tenant leakage | Adversarial isolation tests across data, credentials, media, AI work, and public routes |
| Migration failure | Full-chain replay, restored-backup rehearsal, and Alembic schema-drift checks |
| Billed AI-job ambiguity or duplicate spend | Durable Quote Studio job reconciliation before any provider retry |
| Wrong code released | Exact-commit test receipts and post-deploy API/frontend revision verification |

The [engineering evidence note](docs/engineering-evidence.md) contains the measurement basis, application-language breakdown, test-count boundary, schema snapshot, and release-evidence limits.

---

## Near-term roadmap

### Explainable technical-analysis context

Add basic trend and momentum indicators over completed locally sampled candles, with versioned calculations, traceable inputs, coverage and freshness gates, expiry, and decision-support-only language. Signals will not feed the deterministic offer engine or represent executable trading advice.

### Richer market exploration

Extend the existing interactive line/candle, range, and interval experience with deeper comparison and inspection tools while preserving gaps and provider-delay disclosures.

### AI Rarity Insights

Add a tenant-scoped, allowance-metered workflow for requesting an in-depth review of one coin from a configured foundation model. Query allowances and provider budgets will scale by dealer subscription tier. Outputs will remain guarded, non-authoritative research—not authentication, grading, provenance, rarity certification, or pricing authority.

[Review roadmap principles, gates, and later work →](docs/roadmap.md)

---

## Known limitations

- The current media adapter uses a persistent local object-store volume behind a provider-neutral interface; an R2/S3 adapter is not yet wired.
- Premium confidence is not yet surfaced as a first-class dealer control. Sparse comparable coverage remains visible instead of being hidden behind false certainty.
- Optional AI design capabilities require provider, worker, storage, budget, and operator readiness; the deterministic Quote Studio must remain usable when those lanes are disabled.
- CDN sandbox capability is not a claim of partnership, production authorization, or redistribution rights.

## Stack

Python 3.12 · FastAPI (async) · SQLAlchemy 2.0 · Alembic · PostgreSQL 16 · pytest · React 18 · Vite · Node test runner · Docker Compose · Caddy · DigitalOcean · Cloudflare Pages · Stripe

---

**CounterMint™ is built and operated by [Raymond J. Kraft](https://linkedin.com/in/raymondkraft) through [Roots No Limits LLC](https://rootsnolimits.com).**

[countermint.io](https://countermint.io) · [app.countermint.io](https://app.countermint.io) · [Live customer quote](https://app.countermint.io/q/5eiJMiqsuZgLrZdtKfYuze3W8RrgL6qYuw4bIcIbkvw) · [ray@rootsnolimits.com](mailto:ray@rootsnolimits.com)
