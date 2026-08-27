# CounterMint product tour

[← Back to the project overview](../README.md) · [Quote Studio](quote-studio.md) · [Market intelligence](market-intelligence.md) · [System architecture](architecture.md) · [Engineering evidence](engineering-evidence.md)

These screenshots use demonstration workspaces and test data, not customer records. The current set is dated August 2026 and shows Quote Studio v3. Older private-beta screens are labeled inline where they still explain a current workflow. The staff screenshot has its contact fields permanently redacted.

## 1. Capture the item and preserve catalog identity

<p align="center">
  <a href="../screenshots/vision-intake-segmented.png">
    <img src="../screenshots/vision-intake-segmented.png" width="1000" alt="Earlier private-beta UI showing six graded coins segmented from one photograph into individual staged line items">
  </a>
</p>

**Earlier private-beta UI.** A single photo of six graded slabs becomes six staged items. The system resolves metal, weight, and purity where evidence supports it; missing or uncertain label fields go to dealer confirmation rather than being guessed.

<p align="center">
  <a href="../screenshots/vision-intake-processing.png">
    <img src="../screenshots/vision-intake-processing.png" width="1000" alt="Earlier private-beta UI showing a slab label being processed while current metal spot prices remain visible">
  </a>
</p>

**Earlier private-beta UI.** The intake path keeps current spot, source, and timestamp visible while image processing runs.

<p align="center">
  <a href="../screenshots/cdn-gsid-appraisal.png">
    <img src="../screenshots/cdn-gsid-appraisal.png" width="1100" alt="Standard appraisal intake showing the GSID control that leads into a separate enhanced CDN sandbox workspace">
  </a>
</p>

Catalog identifiers stay distinct. GSID, PCGS catalog number, and Friedberg number describe catalog identity; the certification number remains the holder's unique slab serial. This is the standard appraisal form, not a sandbox screenshot. In sandbox mode, GSID opens a separate enhanced workspace with detailed item insights, protected reference context, independent cross-checks, versioned rule output, staging, and DRAFT review. That additional UI is not pictured publicly; see the [sandbox licensing boundary](cdn-sandbox.md).

## 2. Price melt and collector premium separately

<p align="center">
  <a href="../screenshots/quote-melt-premium-breakdown.png">
    <img src="../screenshots/quote-melt-premium-breakdown.png" width="1000" alt="Earlier private-beta UI showing melt value, collector premium, current comparable value, versioned policy range, and final offer">
  </a>
</p>

**Earlier private-beta UI.** The layout has changed, but the versioned pricing contract is current: each eligible metal-bearing line keeps melt, collector premium, market evidence, dealer-policy range, and final offer separate.

<p align="center">
  <a href="../screenshots/valuation-detail-provenance.png">
    <img src="../screenshots/valuation-detail-provenance.png" width="1000" alt="Earlier private-beta valuation UI showing comparables normalized against spot at each sale date">
  </a>
</p>

**Earlier private-beta UI.** Comparable sales are normalized against spot at their own sale dates. Match scores, inclusion decisions, and the derived premium remain inspectable, and changing the evidence set recalculates deterministically.

<p align="center">
  <a href="../screenshots/comp-outlier-rejection.png">
    <img src="../screenshots/comp-outlier-rejection.png" width="650" alt="Earlier private-beta comparable-sales UI showing included records and reason-coded outlier rejections">
  </a>
</p>

**Earlier private-beta UI.** Automatic removals carry reasons such as poor match, multi-coin lot, or bulk anomaly. A permissioned dealer can override the normal band without erasing the audit trail.

## 3. Explore market history without confusing it with quote authority

Gold, silver, platinum, and palladium stay visible across the dealer workspace. Metals Insights adds 16 other metal instruments in a searchable catalog, for 20 instruments in total.

<p align="center">
  <a href="../screenshots/metals-insights-candles.png">
    <img src="../screenshots/metals-insights-candles.png" width="1100" alt="Metals Insights 24-hour gold candlestick chart with five-minute intervals, zoom and pan controls, a pinned OHLC bucket, coverage, and incomplete-candle disclosure">
  </a>
</p>

The candlestick view supports selectable ranges and intervals, zoom, pan, bucket pinning, previous/next navigation, and detailed OHLC inspection. This 24-hour view uses five-minute buckets and keeps the latest incomplete candle visibly separate from completed history.

<p align="center">
  <a href="../screenshots/metals-insights-line-chart.png">
    <img src="../screenshots/metals-insights-line-chart.png" width="1100" alt="Metals Insights interactive 30-day gold line chart with one-hour intervals, cursor inspection, zoom and pan controls, and provider-delay disclosure">
  </a>
</p>

The line view uses the same native charting path and stored observations. Here, a 30-day range is shown at one-hour intervals with cursor inspection, zoom, pan, and an accessible data table.

Provider observations are delayed by one minute. CounterMint derives candles from the samples it actually stored: first sample for open, observed extrema for high and low, and last sample for close. Empty intervals remain gaps. Metals Insights is dealer research context, not a live trading tool, and it remains separate from the authoritative melt path.

[See the market-series derivation and failure behavior →](market-intelligence.md)

## 4. Make dealer policy explicit and reproducible

<p align="center">
  <a href="../screenshots/pricing-rules-default.png">
    <img src="../screenshots/pricing-rules-default.png" width="1100" alt="Versioned default pricing rule and Premium Protection tiers in CounterMint">
  </a>
</p>

The first matching versioned rule produces the suggested range. Premium Protection reduces only the collector-premium contribution from older comparable sales after a sharp increase in spot; current melt remains intact.

<p align="center">
  <a href="../screenshots/dealer-pricing-rules.png">
    <img src="../screenshots/dealer-pricing-rules.png" width="1000" alt="Earlier private-beta UI for versioned pricing rules with metal, value, grade, key-date, and premium-decay controls">
  </a>
</p>

**Earlier private-beta UI.** The current rules screen appears above. The versioning contract remains the same: saving activates a new policy version, and historical quotes stay tied to the version that priced them.

<p align="center">
  <a href="../screenshots/out-of-band-override.png">
    <img src="../screenshots/out-of-band-override.png" width="1000" alt="Earlier private-beta UI showing an out-of-band offer that requires permission and records an audit event">
  </a>
</p>

**Earlier private-beta UI.** A price outside the suggested band is an override, not a silent edit. Outside-range authority is permission-gated, and attribution preserves who made the decision.

## 5. Design the quote against the real customer renderer

<p align="center">
  <a href="../screenshots/quote-studio-overview.png">
    <img src="../screenshots/quote-studio-overview.png" width="1100" alt="Quote Studio overview showing a design library, structured sections, desktop mobile and print preview, publishing, and gated AI design controls">
  </a>
</p>

The editor is built around the customer quote itself. Dealers choose curated presets, edit drafts, preview safe sample or actual customer-safe data, switch desktop/mobile/print modes, and publish a reviewed design. AI is not enabled in the environment shown; the deterministic editor remains fully usable and sends no provider request.

<p align="center">
  <a href="../screenshots/quote-studio-header-artwork.png">
    <img src="../screenshots/quote-studio-header-artwork.png" width="1050" alt="Quote Studio header artwork editor bound to a structured quote preview">
  </a>
</p>

Clicking the rendered logo, header, customer actions, quote body, or footer opens the schema-approved controls for that region. Header artwork additionally supports bounded dragging, keyboard positioning, locked-proportion resizing, pan, zoom, fit, edge treatment, image adjustments, and overlay.

<p align="center">
  <a href="../screenshots/quote-studio-image-controls.png">
    <img src="../screenshots/quote-studio-image-controls.png" width="420" alt="Expanded Quote Studio image-composition and optimized-derivative controls">
  </a>
</p>

Uploaded or generated artwork begins as a private proposal. CounterMint creates a sanitized, optimized derivative that must be reviewed, accepted into the draft, and separately published.

Capability-gated AI lanes can propose text-free header artwork, symbol-first logo concepts, and approved style-token changes. They cannot alter customer data, quote values, lifecycle state, or publishing state.

[Explore the full structured-design and AI safety model →](quote-studio.md)

## 6. Negotiate end to end

<p align="center">
  <a href="../screenshots/quote-studio-customer-actions.png">
    <img src="../screenshots/quote-studio-customer-actions.png" width="1050" alt="Quote Studio previewing customer accept, counter-offer, and terminal decline controls">
  </a>
</p>

Customer-facing labels are configurable, but lifecycle meaning is not. The server owns accept, customer counter, dealer accept/reject/counter-back, customer response to the dealer counter, and terminal per-item decline. The dealer can configure attempt limits and review thresholds without creating an endless negotiation loop.

<p align="center">
  <a href="../screenshots/customer-quote-spot-drift.png">
    <img src="../screenshots/customer-quote-spot-drift.png" width="620" alt="Earlier private-beta customer quote showing issuance spot, current spot, market drift, and accept or counter actions">
  </a>
</p>

**Earlier private-beta UI.** The customer sees issuance spot, current spot, and drift before responding. Acceptance captures the value offered at that moment. The issued presentation and accepted observation are versioned records; review or a new counter never overwrites the customer's earlier action.

## 7. Govern staff authority and public attribution

<p align="center">
  <a href="../screenshots/staff-roles-attribution-redacted.png">
    <img src="../screenshots/staff-roles-attribution-redacted.png" width="1100" alt="Redacted Staff and Roles workspace showing display names, role hierarchy, permissions, activity, status, and management controls">
  </a>
</p>

Staff & Roles separates internal identity, dealer-approved quote display name, role, pricing authority, activity, and account status. Invitations, deactivation, and reactivation remain tenant-scoped; outside-range decisions and quote/pricing attribution remain auditable.

Public attribution is dealer opt-in and exposes only the approved nickname or first-name fallback—not legal contact details. Every email address and phone number in this screenshot was permanently covered in the exported raster and verified absent by OCR before publication.

---

[View the live customer quote](https://app.countermint.io/q/YcQk3L7mU5mVaF5L-es1bGU8pUIZy7_JVTSQmR-gBOg) · [Review the architecture](architecture.md) · [Review the roadmap](roadmap.md) · [Request a walkthrough](mailto:ray@rootsnolimits.com)
