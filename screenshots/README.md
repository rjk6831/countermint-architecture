# Product screenshot assets

The published narrative lives in the [full product tour](../docs/product-tour.md); the root [project overview](../README.md) uses a curated subset of these images.

## New August 2026 product evidence

| Asset | Demonstrates |
|---|---|
| `quote-studio-overview.png` | Design library, structured editing, responsive preview, publish separation, and capability-gated AI design |
| `quote-studio-header-artwork.png` | Quote-bound header composition, focal controls, and optimized-derivative workflow |
| `quote-studio-image-controls.png` | Frame, fit, anchors, pan, zoom, image adjustments, overlay, and two-step image acceptance |
| `quote-studio-customer-actions.png` | Configurable action labels over fixed accept/counter/terminal-decline semantics |
| `pricing-rules-default.png` | Versioned default offer range and collector-premium protection after spot movement |
| `cdn-gsid-appraisal.png` | Standard appraisal intake and the GSID entry point; this is not a capture of the separate enhanced sandbox UI |
| `staff-roles-attribution-redacted.png` | Role hierarchy, status, pricing authority, activity, and quote-display attribution with every contact field permanently redacted |

The unsanitized staff source is intentionally not stored in this repository. The published derivative uses opaque raster redaction; an OCR pass confirmed that no email address or phone number remains readable.

## Earlier private-beta captures

The files below predate the August 2026 UI set. Documentation captions label them **Earlier private-beta UI** so a reader does not mistake the old layout for the current product. They remain because the workflow or versioning rule they demonstrate is still part of CounterMint.

| Asset | Still-current contract |
|---|---|
| `vision-intake-segmented.png` | One image can stage multiple items for review |
| `vision-intake-processing.png` | Intake keeps market source and freshness visible while processing |
| `quote-melt-premium-breakdown.png` | Melt, collector premium, market evidence, versioned policy range, and offer remain separate |
| `valuation-detail-provenance.png` | Comparable normalization and inclusion decisions remain inspectable |
| `comp-outlier-rejection.png` | Automated removals are reason-coded and auditable |
| `dealer-workspace-settings.png` | Dealer settings remain tenant-scoped; the current navigation and layout differ |
| `dealer-pricing-rules.png` | Saving a rule set activates a new version instead of rewriting quote history |
| `out-of-band-override.png` | Pricing overrides require authority and attribution |
| `customer-quote-spot-drift.png` | Issued and accepted observations remain distinct, durable records |
| `staff-seats-rbac.png` | Staff permissions remain role- and tenant-scoped; the current Staff & Roles UI is shown above |

## Publication rules

Before committing an image, remove or replace:

- tenant, business, staff, and customer identities that are not cleared demonstration data;
- real or unapproved email addresses, phone numbers, and messaging IDs;
- API keys, tokens, private URLs, and internal infrastructure details;
- real transaction values that are not intended for publication;
- provider-returned fields that are not permitted on a public surface.

Use a controlled demonstration tenant. Wide, text-heavy screenshots should remain full width in the documentation so labels stay legible; readers can open the source image for full resolution.

## Why screenshots remain PNG

A browser screenshot is raster data. Wrapping it in SVG does not make its text or controls vector; it only embeds the same bitmap, typically with more complexity and no fidelity gain. An SVG overlay is also unsafe for privacy redaction because the original pixels remain extractable beneath the overlay.

Public screenshots therefore remain sanitized PNGs with metadata removed. True SVG is reserved for authored architecture diagrams such as the platform topology and pricing flow.

## Capture checklist

- Capture the complete state needed to support the caption.
- Keep source and timestamp visible when the claim depends on market freshness.
- Do not crop away warnings, pending states, or evidence that changes the meaning of the screen.
- Write alt text that names the decision or workflow being demonstrated, not only the visual layout.
- Burn redaction into exported pixels, strip metadata, and OCR-check contact-sensitive assets.
- Recheck every screenshot when UI copy or public behavior changes.
