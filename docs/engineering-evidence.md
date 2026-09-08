# Engineering evidence

[← Back to the project overview](../README.md)

CounterMint's application repository is private. This page records the basis for the project-size and test figures quoted in the public documentation, and keeps inventory, verification, and production state separate.

## Repository measurement

The figures in the README are generated. [`metrics/collect.mjs`](../metrics/collect.mjs) walks `git ls-files` at the product's HEAD, classifies every tracked file by path and extension, and writes [`metrics/latest.json`](../metrics/latest.json); [`metrics/render.mjs`](../metrics/render.mjs) rewrites the README table from that file. A GitHub Actions workflow in the private repository runs both on every push to `main`. The counting rules and the way to reproduce a measurement by hand are in [`metrics/README.md`](../metrics/README.md).

What the categories contain for this product:

| Category | Contents |
|---|---|
| Application source | `api/app` (Python: FastAPI, SQLAlchemy 2.0, pricing engine, provider gateway, media, billing), `api/scripts`, `web/src` (React 18 dealer SPA and customer quote surface), and small JSON configuration |
| Automated tests | `api/tests` (pytest against PostgreSQL) and `web/tests` (Node test runner), including fixtures and recorded provider responses |
| Migrations | `api/alembic/versions`, one file per revision |
| Infrastructure | `deploy/`, Docker Compose files, the API Dockerfile, GitHub workflows, `pyproject.toml`, `alembic.ini`, build and Vite configuration, shell scripts |
| Excluded | The recorded benchmark evidence and demo showcase datasets under `docs/evidence` and `api/app/demo_showcase/data` (JSON over 2,000 lines), binaries, lockfiles, generated build output |

The marketing site at countermint.io lives in its own repository and deploys as a Cloudflare Worker. It is not included in the product figures.

## Automated-test claim

The README reports **declared test cases**: `def test_` functions in pytest files plus `test(` and `it(` calls in the Node test-runner files. `@pytest.mark.parametrize` decorators are counted once here and expand when pytest collects, so a pytest run reports a larger number than the README does. The README number is the floor.

The declared count is an inventory of what exists in source at the named commit. An exact passing result belongs to a dated test run against a specific commit, which is what the release path below produces.

Coverage includes:

- pricing arithmetic, historical-spot normalization, outlier handling, and pricing-drift invariants;
- quote state transitions, counters, expiry behavior, and append-only acceptance history;
- authentication, staff roles, seat limits, tenant isolation, and credential handling;
- PostgreSQL models, Alembic constraints, upgrade behavior, and schema-drift checks;
- market-data, sold-comparable, Greysheet, billing, email, and AI-provider contracts through recorded or mocked responses;
- frontend API adapters, customer quote presentation, settings, staff controls, Quote Studio state, and visual-baseline contracts.

## Schema and release controls

The README migration count is the number of Alembic revision files in source at the named commit. It says nothing about which revision production is at; that belongs to the deploy receipt.

The release path is built around an exact candidate commit:

1. Run backend and frontend verification against the candidate.
2. Rehearse the full migration chain in disposable PostgreSQL, including a restored production-backup path, then check for model and schema drift.
3. Build the frontend and container image from the tested source.
4. Stamp the candidate commit into the deployable artifact.
5. Apply forward migrations, recreate services, and verify health.
6. Confirm the API and frontend report the expected commit.

These controls reduce release risk. They are not presented as proof of uptime, provider availability, or a current production deployment unless a separate live receipt exists.

## Evidence boundaries

| Claim type | What this repository can support |
|---|---|
| Architecture | Current system boundaries, deliberate trade-offs, and separately labeled planned work |
| Product behavior | Redacted demo screenshots and a public customer-quote path |
| Repository scale | Generated, dated file and line measurements with the inclusion basis above |
| Test inventory | Declared test cases at a named commit, and the coverage areas above |
| Production state | Preview status only; live deployment claims require separate runtime evidence |
| Provider state | Adapter or fixture completion is kept distinct from authorized live-provider access |

That separation is deliberate. A local test, a recorded provider fixture, a browser screenshot, and a production health check answer different questions and should not be collapsed into one "verified" label.

## Selected high-risk controls

- A PostgreSQL advisory lock keeps spot polling singleton across API replicas.
- The internal CDN sandbox uses a secretless tenant entitlement row and a separately held deployment token; request secrets are redacted from HTTP and exception logs. Dealer-supplied BYOK credential envelopes, if commercially authorized for activation, use AES-256-GCM.
- Provider-returned raw wholesale values follow provider-specific retention and purge rules while safe attribution is retained.
- Durable AI jobs reserve budget before dispatch and reconcile ambiguous outcomes before any retry of a billable request.
- Deployment requires an exact-commit test receipt and verifies the running API and frontend revision after service recreation.
