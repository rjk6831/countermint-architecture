# Quote Studio: structured design for a live financial document

[← Back to the project overview](../README.md) · [Product tour](product-tour.md) · [System architecture](architecture.md) · [Roadmap](roadmap.md)

Quote Studio turns quote presentation into versioned application state. Dealers can shape the customer experience around real quote data without introducing arbitrary HTML, CSS, or an unconstrained page builder.

<p align="center">
  <a href="../screenshots/quote-studio-overview.png">
    <img src="../screenshots/quote-studio-overview.png" width="1100" alt="Quote Studio overview with design library, responsive preview, structured editing sections, publishing controls, and gated AI-assisted design">
  </a>
</p>

The captured environment shows the intended fallback posture: AI generation is not enabled there, but the deterministic editor, preview, and publication workflow remain usable. Optional providers extend the Studio; they are not required for it to function.

## What the dealer controls

| Surface | Current capability | Guardrail |
|---|---|---|
| Design library | Create from curated presets, duplicate a design, retain active and draft identities | Designs and revisions remain tenant-owned |
| Preview | Render safe sample data or an actual customer-safe quote in desktop, mobile, and print modes | Preview adapters cannot expose private quote or tenant fields |
| Look and type | Approved palettes, typography, spacing, hierarchy, and layout options | Values must satisfy the versioned design schema |
| Brand and logo | Select accepted dealer-owned assets and composition options | CounterMint product branding is never treated as tenant media |
| Header artwork | Frame, fit, anchors, pan, zoom, edge treatment, adjustments, overlay, and bounded direct manipulation | Artwork remains inside its assigned quote region |
| Quote body | Schema-approved presentation choices around authoritative quote values | Presentation cannot rewrite pricing data |
| Customer actions | Curated labels and optional terminal item-decline presentation | Labels do not redefine lifecycle semantics |
| Footer | Dealer presentation and contact treatment inside the approved contract | Issued quotes preserve their resolved presentation |
| Publish | Activate a reviewed version separately from draft editing | Publishing is explicit; AI output cannot publish itself |

## Click-to-edit, JSON-backed behavior

The large quote preview is the editor's navigation surface. Clicking its logo, header, customer-actions area, quote body, or footer selects that region and opens only the controls allowed by the `QuoteDesignDocument` schema.

This creates a useful separation:

- **Rendered object:** what the dealer sees and selects.
- **Validated design field:** the structured value that actually changes.
- **Shared renderer:** the component that resolves safe sample, private preview, customer, and print modes.
- **Versioned revision:** the persisted draft that supports optimistic concurrency rather than last-write-wins editing.
- **Presentation snapshot:** the resolved design and assets attached to an issued quote so later branding changes do not rewrite history.

Direct spatial manipulation is intentionally bounded. Header artwork supports pointer dragging, keyboard positioning, and locked-proportion corner resizing with normalized geometry persisted into the design document. Other quote regions are clickable and schema-bound; they are not all freely draggable objects.

## Artwork composition without a Photoshop-sized control surface

<p align="center">
  <a href="../screenshots/quote-studio-header-artwork.png">
    <img src="../screenshots/quote-studio-header-artwork.png" width="1050" alt="Quote Studio composing header artwork with frame, fit, pan, zoom, edge, adjustment, and optimized-image controls">
  </a>
</p>

The dealer can work from the quote itself while adjusting:

- standard or bounded frame treatment;
- contain/fill behavior and focal anchors;
- horizontal and vertical pan;
- zoom with normalized geometry;
- solid or image-derived edge treatment;
- brightness, contrast, and saturation;
- an optional color overlay;
- a two-step optimized-derivative review before the image enters the design.

<p align="center">
  <a href="../screenshots/quote-studio-image-controls.png">
    <img src="../screenshots/quote-studio-image-controls.png" width="420" alt="Expanded Quote Studio image controls for framing, fit, anchors, pan, zoom, adjustments, overlay, and optimized derivative review">
  </a>
</p>

These controls are deliberately progressive. A dealer can start from a preset and make a few high-value decisions without confronting hundreds of low-level style knobs; advanced controls remain available when composition needs them.

## The media lifecycle is part of the security model

Uploaded and generated media does not become public merely because it exists.

1. The original remains private and tenant-fenced.
2. CounterMint validates MIME/signature agreement and rejects malformed, animated, or oversized raster input.
3. A bounded derivative strips unsafe metadata and normalizes the asset for its intended role.
4. The dealer previews and explicitly accepts the derivative into a design draft.
5. Publishing activates the reviewed design.
6. Quote issuance resolves accepted assets into the quote's presentation snapshot.

Legacy SVG uploads are not accepted into the new dealer-media lane. Public output is sanitized or rasterized and delivered with defensive response headers.

## AI-assisted quote design, with authority kept narrow

Quote Studio implements three capability-gated AI lanes:

- **Text-free header artwork** for a quote-safe visual region.
- **Symbol-first logo concepts** that remain private proposals.
- **Bounded style proposals** over approved colors, type, spacing, and layout tokens.

The AI does not receive authority to alter customer data, quote values, pricing evidence, lifecycle behavior, or publication state. A generated image must pass the same derivative review as an upload. A style proposal is an inert structured diff until an authorized dealer explicitly applies it; publishing remains a separate action.

Provider work is guarded by readiness, tenant idempotency, reservation, worker lease/heartbeat, provider-start fencing, budget control, reconciliation of ambiguous outcomes, and settle-once accounting. Unknown billable outcomes enter reconciliation instead of being blindly retried.

This means the accurate product claim is **AI-assisted quote design**, not autonomous template publishing. Canonical templates remain curated platform contracts.

## Customer actions: configurable presentation, fixed meaning

<p align="center">
  <a href="../screenshots/quote-studio-customer-actions.png">
    <img src="../screenshots/quote-studio-customer-actions.png" width="1050" alt="Quote Studio customer-action controls previewing accept, counter-offer, and terminal per-item decline states">
  </a>
</p>

Dealers can choose approved customer-facing labels and whether a terminal item-decline action is offered. Underneath those labels, CounterMint retains fixed semantics for:

- accepting the original offer;
- submitting a customer counter;
- accepting or rejecting a dealer counter;
- declining one item terminally while leaving other quote lines available.

The preview can move through lifecycle states so the dealer sees more than the happy path before publishing. Issued quotes freeze the selected presentation, while all transitions remain server-validated and audited.

## Status boundary

| State | Scope |
|---|---|
| **Implemented** | Versioned design documents and revisions, presets, contextual editing, shared renderer, responsive/print preview, bounded header manipulation, raster asset pipeline, customer-action presentation, explicit publish, issued presentation snapshots |
| **Implemented but capability-gated** | Header-artwork generation, logo-concept generation, and bounded AI style proposals |
| **Environment-specific release evidence required** | Provider activation, worker readiness, budget policy, storage readiness, and live end-to-end provider proof |
| **Deliberately not claimed** | Arbitrary CSS, a freeform canvas, AI-authored quote values, autonomous publishing, or universal provider availability |

[Continue through the full product tour →](product-tour.md)
