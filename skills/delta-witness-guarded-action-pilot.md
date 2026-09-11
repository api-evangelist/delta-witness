---
name: delta-witness-guarded-action-pilot
description: >-
  Buy one bounded $10 evidence package: witness 1-3 public HTTPS pages as
  timestamped DELTA proofs and optionally run one deterministic preflight check.
api: DELTA Witness API
operations:
  - quote
  - guardedActionPilot
  - verifyProof
generated: '2026-09-11'
method: generated
source: >-
  Grounded in real operationIds from openapi/delta-witness-openapi.json (v0.7.0).
---

# Guarded-Action Pilot ($10 flat)

Use this for a higher-value autonomous action that needs an evidence bundle over
several public sources, with no subscription.

## Steps

1. **Confirm price (free).** `GET /v1/quote?product=guarded-action-pilot` →
   exactly `$10` USDC on Base, `max_urls: 3`.
2. **Run the pilot.** `POST /v1/guarded-action-pilot` (operationId
   `guardedActionPilot`) with:
   - `urls` — 1 to 3 public **HTTPS** pages to witness (required).
   - `preflight` — optional `{url_index, expected}` to run one deterministic
     check against one of the submitted URLs.
   Pay the single exact $10 x402 v2 payment when challenged (402).
3. **Collect proofs.** The 200 response returns `proofs[]` (each with `url`,
   `proof_id`, `manifest_url`, `public_proof_url`, `bundle_root`, `observed_at`,
   `allocated_price_usd`), an optional `preflight` verdict, and an `economics`
   block.
4. **Verify (free).** `GET /v1/proofs/{proof_id}` for each proof.

## Rules

- Exactly $10; the payment is upfront and idempotent on identical replay.
- All target URLs must be public HTTPS (the pilot rejects non-HTTPS).
- DELTA proves observation and change, not source truth.
