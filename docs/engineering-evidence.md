# Engineering evidence

[← Back to the project overview](../README.md)

This note defines the public scale and verification claims used in the CounterMint architecture portfolio. The application repository is private, so the measurement boundary matters as much as the headline.

## Repository measurement

**Snapshot date:** August 24, 2026

| Scope | Files | Lines | Notes |
|---|---:|---:|---|
| Git-visible working tree | 472 | — | Includes text and binary assets |
| All text files | 456 | 95,180 | Broadest text measurement |
| Selected source, configuration, and documentation | 430 | 93,506 | Text source, configuration, and documentation set |
| Public headline basis | **429** | **92,545** | Maintained project footprint used for the public headline |
| Binary assets | 16 | — | Measured separately from text lines |

The README rounds the headline to **92.5K+ measured project lines**. It does not describe all 92,545 lines as application source code; the total includes source, tests, configuration, scripts, SQL, and documentation.

### Language and file-type breakdown

| Type | Files | Lines | Share of measured total |
|---|---:|---:|---:|
| Python | 250 | 59,508 | 64.3% |
| Markdown | 61 | 12,094 | 13.1% |
| JSX | 33 | 10,057 | 10.9% |
| JavaScript | 56 | 6,472 | 7.0% |
| JSON | 11 | 1,166 | 1.3% |
| CSS | 1 | 1,159 | 1.3% |
| Shell | 9 | 1,095 | 1.2% |
| SQL | 1 | 630 | 0.7% |
| YAML | 3 | 267 | 0.3% |
| TOML | 1 | 49 | 0.1% |
| MJS | 2 | 34 | <0.1% |
| HTML | 1 | 14 | <0.1% |
| **Total** | **429** | **92,545** | **100%** |

For a language-only comparison, Python plus JavaScript and JSX account for **76,037 lines**. Both figures are published here so a reviewer can distinguish the application-language footprint from the broader maintained project footprint.

## Automated-test claim

The public claim is deliberately conservative: **700+ automated backend and frontend tests**.

It is written as a threshold instead of an exact collection result because the repository includes parameterized cases, PostgreSQL-backed integration suites, and environment-sensitive provider fixtures. An exact “passing” count belongs to a dated, exact-commit test receipt. This page does not convert a source-level inventory into a claim that every case passed in this documentation refresh.

Coverage includes:

- pricing arithmetic, historical-spot normalization, outlier handling, and pricing-drift invariants;
- quote state transitions, counters, expiry behavior, and append-only acceptance history;
- authentication, staff roles, seat limits, tenant isolation, and credential handling;
- PostgreSQL models, Alembic constraints, upgrade behavior, and schema-drift checks;
- market-data, sold-comparable, Greysheet, billing, email, and AI-provider contracts through recorded or mocked responses;
- frontend API adapters, customer quote presentation, settings, staff controls, Quote Studio state, and visual-baseline contracts.

## Schema and release controls

At the August 24 measurement point, the working tree contained **41 Alembic migration files**: **40 tracked at the measured commit** and one active untracked revision. The README retains the conservative **40+** headline without presenting that active application work as released history.

The release path is designed around an exact candidate rather than an untracked “latest” state:

1. Run backend and frontend verification against the candidate.
2. Rehearse the full migration chain in disposable PostgreSQL, including a restored production-backup path, then check for model/schema drift.
3. Build the frontend and container image from the tested source.
4. Stamp the candidate commit into the deployable artifact.
5. Apply forward migrations, recreate services, and verify health.
6. Confirm the API and frontend report the expected commit.

These controls reduce release risk; they are not presented as proof of uptime, provider availability, or a current production deployment unless a separate live receipt exists.

## Evidence boundaries

| Claim type | What this repository can support |
|---|---|
| Architecture | Current system boundaries, deliberate tradeoffs, and separately labeled planned work |
| Product behavior | Redacted demo screenshots and a public customer-quote path |
| Repository scale | Dated file and line measurements with the inclusion basis above |
| Test inventory | A conservative threshold and named coverage areas |
| Production state | Private-beta status only; live deployment claims require separate runtime evidence |
| Provider state | Adapter or fixture completion is kept distinct from authorized live-provider access |

That separation is deliberate. A local test, a recorded provider fixture, a browser screenshot, and a production health check answer different questions and should not be collapsed into one “verified” label.

## Selected high-risk controls

- A PostgreSQL advisory lock keeps spot polling singleton across API replicas.
- The internal CDN sandbox uses a secretless tenant entitlement row and a separately held deployment token; request secrets are redacted from HTTP and exception logs. Dealer-supplied BYOK credential envelopes, if commercially authorized for activation, use AES-256-GCM.
- Provider-returned raw wholesale values follow provider-specific retention and purge rules while safe attribution is retained.
- Durable AI jobs reserve budget before dispatch and reconcile ambiguous outcomes instead of blindly retrying a billable request.
- Deployment requires an exact-commit test receipt and verifies the running API/frontend revision after service recreation.

## AI-assisted development

AI accelerates specification, implementation, review, and adversarial test generation. Accountability remains human: architecture, domain rules, security boundaries, provider contracts, migration safety, review decisions, and deployment operation are owned by Raymond J. Kraft.

The quality claim is therefore not “AI wrote a large codebase.” It is that high-throughput implementation is constrained by explicit invariants, reproducible tests, forward-only schema discipline, and evidence that states exactly what was—and was not—verified.
