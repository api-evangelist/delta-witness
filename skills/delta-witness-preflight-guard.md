---
name: delta-witness-preflight-guard
description: >-
  Verify that a public web source still says what you expect before an agent
  takes a consequential action, and keep a timestamped proof of what was seen.
api: DELTA Witness API
operations:
  - quote
  - preflightAutonomousAction
  - verifyProof
generated: '2026-09-11'
method: generated
source: >-
  Grounded in real operationIds from openapi/delta-witness-openapi.json (v0.7.0)
  plus conventions/errors/idempotency artifacts in this repo.
---

# Preflight guard before a consequential action

Use this when an autonomous action depends on what a public page says *now*
(a price, a policy, a published statement, a stock state).

## Steps

1. **Budget (optional).** `GET /v1/quote?product=preflight` to confirm the
   current price ($5 USDC on Base). Free, no payment.
2. **Guard.** `POST /v1/preflight` (operationId `preflightAutonomousAction`) with:
   - `url` — the public HTTP(S) source to observe (required).
   - `expected` — deterministic expectations: `contains[]`, `excludes[]`,
     `html_sha256`, or `markdown_sha256`.
   - `prior_proof_id` — optional DELTA proof UUID to use as the hash baseline.
   - `freshness_seconds` — optional max acceptable age.
   Settle the x402 v2 payment (USDC on Base, upfront) when challenged with 402.
3. **Read the verdict.** The 200 response returns `safe`, `changed`, `reason`,
   `diff`, plus a `proof_id`, `manifest_url` and `public_proof_url`.
   `safe: true` means the supplied deterministic expectations matched — it does
   **not** assert the source is truthful, only that it matched what you declared.
4. **Act or abort** based on `safe`/`changed`.
5. **Verify later (free).** `GET /v1/proofs/{proof_id}` (operationId
   `verifyProof`) returns the public proof metadata and hashes; raw artifacts
   stay private.

## Rules

- Payment is upfront x402 v2; an identical settled payment replays the same
  fulfillment (`idempotent_replay: true`). A replay with a different body → 409.
- Only public HTTPS targets are accepted; private/link-local/credential-bearing
  targets are rejected (400).
- A 502 means the settled observation failed; retry with the identical request —
  it stays idempotent, so you are not double-charged.
