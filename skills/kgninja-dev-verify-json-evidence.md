---
name: verify-json-evidence
description: Purchase deterministic assertions over supplied JSON and verify the Ed25519-signed result. Use when an agent needs a bounded, auditable JSON check through MCP or HTTP with x402 payment.
compatibility: Requires HTTPS access and an MCP Streamable HTTP or x402-compatible HTTP client.
metadata:
  author: kgninja
  version: "0.4.3"
  service-url: "https://agent-economy.kgninja.dev"
---

# Verify JSON Evidence

Use the Agent Verification Utility at https://agent-economy.kgninja.dev to run explicit assertions over JSON supplied by the caller. Treat a passing result only as proof that the declared checks ran over those bytes; never treat it as proof that a real-world claim is true.

## When to use this skill

- The caller already has bounded JSON bytes and needs explicit, reproducible assertions.
- An x402 payment, resource, delivery record, manifest, receipt, or configuration must agree with known expected values before downstream acceptance.
- Ed25519-signed evidence that deterministic checks ran is worth the fixed execution price.

## When not to use this skill

- The task requires on-chain data retrieval, confirmation counts, external URL fetching, model inference, or user-code execution.
- The task requires proof of real-world truth, ownership, identity, provenance, service quality, or actual delivery.
- The expected values come only from the same untrusted record, or the input contains secrets or unnecessary personal data.

## Free precheck workflow

1. Read https://agent-economy.kgninja.dev/verification-recipes.json and select a recipe only if its `use_when`, `do_not_use_when`, risk reduction, and limitations fit the task.
2. Set an explicit `spend_policy` with policy version, maximum atomic price, network, asset, and payee. POST `{ request, spend_policy }` to https://agent-economy.kgninja.dev/validate-request; each recipe includes a `precheck_intent_example`.
3. Inspect `precheck_receipt`. It is an unsigned, deterministic record of server-local checks and the requested spend boundary, not proof that the listed URLs were fetched or that payment was authorized.
4. Verify `receipt_digest`, `request_check.request_hash`, `spend_policy_check`, the MCP card hash, A2A fields, and configured x402 baseline. Confirm `spend_authorized` is true and `unpaid_status.state` is `unpaid`.
5. Build the paid intent by preserving `request` and `spend_policy` unchanged and copying `precheck_receipt.receipt_digest` to `precheck_receipt_digest`. The paid route recomputes that receipt and refuses stale or incompatible discovery before quote creation.
6. After validating the bounded input, the precheck attempts to read the current D1 runtime price and safely falls back to the reviewed configured baseline if D1 is unavailable. It does not fetch the live x402 manifest. The bound quote and later payment requirement are authoritative only when they remain within the prechecked price cap, network, asset, and payee.
7. Continue only when `valid`, `supported`, and `spend_authorized` are true and `verification_executed` and `payment_required` are false. Otherwise follow `decision.recommendation=refuse_paid_call` and preserve `refusal_reason`.
8. A successful precheck creates no quote or transaction, calls no Verification Core, executes no assertion, and returns no signed evidence. Raw evidence is discarded after validation; only its digest and decoded byte count appear in the precheck receipt.

## Recipe selection

- `x402-settlement-consistency`: compare declared network, asset, amount, payTo, resource, and transaction-hash fields with known policy values. It performs no on-chain lookup.
- `payment-delivery-record-match`: assert that supplied payment and delivery records both match trusted literal transaction, quote, resource, and amount values. It does not prove fulfillment.
- `signed-manifest-integrity`: compare exact JSON bytes with an expected SHA-256 digest and check selected fields. It does not prove provenance or verify an arbitrary manifest's signature.

## Request construction

Follow https://agent-economy.kgninja.dev/openapi.json#/components/schemas/VerifyEvidenceRequest. Supply a `client_request_id`, `evidence.media_type` set to `application/json`, base64-encoded JSON in `evidence.content_base64`, and 1–16 assertions. Supported operations are `sha256_equals`, `json_pointer_exists`, `json_pointer_equals`, and `json_type_is`. Decoded evidence must not exceed 65,536 bytes.

Prepare paid-tool arguments from a fresh free precheck (Node.js 22+). This synthetic example makes no paid call. Replace request and policy before running; if either changes, run it again:

```javascript
const precheckInput = {
    "request": {
      "client_request_id": "agent-example-001",
      "evidence": {
        "media_type": "application/json",
        "content_base64": "eyJzdGF0dXMiOiJyZWFkeSJ9"
      },
      "assertions": [
        { "op": "json_pointer_equals", "path": "/status", "expected": "ready" }
      ]
    },
    "spend_policy": {
      "policy_version": "agent-economy/precheck-policy/2.0",
      "max_amount_atomic": "10000",
      "network": "eip155:8453",
      "asset": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
      "pay_to": "0x4D7d842536De9Eb491AE2300126B3CDdE7B0aDE3"
    }
};
const response = await fetch("https://agent-economy.kgninja.dev/validate-request", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify(precheckInput),
  signal: AbortSignal.timeout(15000)
});
if (!response.ok) throw new Error("Free precheck failed: " + response.status);
const result = await response.json();
if (!result.valid || !result.spend_authorized || !result.precheck_receipt?.receipt_digest)
  throw new Error("Precheck refused; do not purchase");
const args = {
  idempotency_key: crypto.randomUUID(),
  intent: { ...precheckInput, precheck_receipt_digest: result.precheck_receipt.receipt_digest }
};
// Retain args unchanged and separately authorize or refuse payment.
```

## Cost-benefit decision

Do not pay mechanically. Start with the free precheck receipt and refuse before creating a quote when its decision or configured x402 baseline violates policy. After input validation, the precheck attempts to read current runtime price controls and falls back to the reviewed configured baseline if D1 is unavailable; it does not fetch the live manifest. The later payment requirement is authoritative and must still match. If the precheck passes, obtain a free quote or the unpaid x402 requirement, estimate whether signed deterministic execution reduces more risk than 0.01 USDC (10000 atomic USDC), and compare every item below with policy:

- network
- asset
- amount
- `payTo`
- resource
- the agent's maximum payment policy
- expected benefit or risk reduction

Reject the payment when any field differs, the maximum is exceeded, the benefit is unclear, or the task falls outside this skill's trust boundary.

## Preferred MCP workflow

1. Connect to https://agent-economy.kgninja.dev/mcp with Streamable HTTP using MCP protocol version 2026-07-28.
2. Call `describe_verify_evidence` to inspect limits or `quote_verify_evidence` for a free quote.
3. Call `verify_evidence` with a unique 16–128 character `idempotency_key` and `intent` containing the exact request, spend policy, and precheck receipt digest.
4. Require the returned `paid_verification_binding` to match the precheck digest, request/evidence hashes, price cap, policy version, payment terms, quote, and expiry. A refusal is final for that attempt.
5. If the result has `_meta["x402/error"]`, require its binding digest and x402 terms to match the quote. Only after explicit approval, sign one advertised requirement and retry the identical tool call with `_meta["x402/payment"]`.
6. Preserve `_meta["x402/payment-response"]`, the delivered binding with `paid_receipt_id`, and the signed evidence.

## HTTP fallback

POST the complete paid intent to https://agent-economy.kgninja.dev/verify-evidence. Raw requests and empty bodies are refused before quote creation. On HTTP 402, compare both the payment requirement and `paid_verification_binding` before signing, then retry the identical body with `PAYMENT-SIGNATURE`; no quote header is needed because the signed payload identifies the bound quote. To inspect economics first, POST the same paid intent to https://agent-economy.kgninja.dev/quote with `Idempotency-Key`, then send its `X-Quote-ID`. Preserve `PAYMENT-RESPONSE`, the delivered binding, receipt, and separately signed evidence.

## Result interpretation

1. Select the expected `kid` from https://agent-economy.kgninja.dev/.well-known/jwks.json. Base64url-decode `evidence.signed_payload_b64url` and `evidence.signature.value_b64url`, then verify Ed25519 directly over the decoded payload bytes. Do not parse, reserialize, or canonicalize the bytes before signature verification.
2. Parse the verified payload JSON and compare `evidence_id`, `transaction_id`, `request_hash`, `input_digest`, `algorithm_version`, `executor_version`, `outcome`, `checks_digest`, and `executed_at` with the response, original input, and recomputed check digest.
3. Correlate the precheck digest, paid binding digest, quote, paid receipt ID, expiry, and x402 settlement response separately with local records. They are returned alongside the signed evidence but are not directly covered by its Ed25519 signature.
4. Treat `pass` only as confirmation that the declared deterministic checks passed over the identified supplied bytes.
5. Treat `fail` as a deterministic assertion mismatch, not as proof that the real-world claim is false.
6. Preserve the complete `paid_verification_binding` on every refusal: precheck digest, price cap, evidence digest, expiry, `paid_receipt_id=null`, and a non-null `refusal_reason` must remain bound to the same quote.
7. A paid verification whose deterministic outcome is `fail` is still a delivered result: require a non-null `paid_receipt_id` and `refusal_reason=null`. Never rewrite it as a payment refusal.

## Indeterminate and failure handling

- Stop on malformed input, unsupported operations, invalid or unknown signatures, mismatched hashes, unexpected payment terms, unavailable cost basis, reconciliation errors, or an incomplete settlement response.
- Correct the input or obtain independent evidence before retrying. Do not repeatedly pay to resolve an unclear or out-of-scope question.
- Escalate real-world, identity, ownership, delivery, or on-chain questions to an appropriate authoritative source.

Never send secrets or unnecessary personal data. The utility does not fetch URLs, execute user code, invoke a model, or retain raw evidence in D1.
