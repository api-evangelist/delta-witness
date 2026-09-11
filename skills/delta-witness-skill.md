---
name: delta-witness
description: Observe public web sources and guard consequential autonomous actions with timestamped hashes.
homepage: https://delta-witness-api.ruphussten.workers.dev
---

# DELTA Witness

Use DELTA when an action depends on what a public source says now.

- Capture: `POST https://delta-witness-api.ruphussten.workers.dev/v1/capture` with `{"url":"https://example.com"}`.
- Guard: `POST https://delta-witness-api.ruphussten.workers.dev/v1/preflight` with a URL plus a prior proof, expected hash, or textual expectations.
- $10 Guarded-Action Pilot: `POST https://delta-witness-api.ruphussten.workers.dev/v1/guarded-action-pilot` with 1-3 public HTTPS URLs and an optional deterministic preflight on one URL. No subscription.
- Watch: available only through authenticated prepaid partner gateways; quota is finite and every scheduled check must remain margin-positive.
- Payment: x402 v2 on Base mainnet USDC, settled before capture work.
- Semantics: `safe` means supplied deterministic expectations matched. DELTA proves observation/change, not source truth.

Read `https://delta-witness-api.ruphussten.workers.dev/openapi.json` for exact schemas and `https://delta-witness-api.ruphussten.workers.dev/docs` for client flow.
