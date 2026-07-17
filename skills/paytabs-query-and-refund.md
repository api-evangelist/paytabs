---
generated: '2026-07-17'
method: generated
name: Query a transaction and refund it
description: Look up a prior PayTabs transaction and issue a full or partial refund.
api: openapi/paytabs-openapi.yml
operations: [queryTransaction, createPaymentRequest]
source: >-
  operationIds verified in openapi/paytabs-openapi.yml. Refund is a follow-up
  tran_type on POST /payment/request referencing a prior tran_ref.
---

# Query a transaction and refund it

Refunds move money in reverse — keep a human in the loop (`agentic-access/paytabs-agentic-access.yml`).

## Auth
- Merchant **server key** in the `authorization` header, on the profile's region host. See `authentication/paytabs-authentication.yml`.

## Steps
1. **Find the transaction** — `queryTransaction` (`POST /payment/query`) with `profile_id` and either `tran_ref` (single) or `cart_id` (array for the cart). Confirm it is authorized/settled (`payment_result.response_status = A`) before refunding.
2. **Refund** — `createPaymentRequest` (`POST /payment/request`) with `tran_type: refund`, the original `tran_ref`, and `cart_currency` matching the original (a currency mismatch is rejected with response code 206). Omit/echo the full `cart_amount` for a full refund or pass a smaller `cart_amount` for a partial refund.
3. **Verify** — the response `payment_result.response_status` is `P` (pending) or a terminal status; a refund IPN/callback confirms completion. See `asyncapi/paytabs-webhooks-asyncapi.yml`.

## Idempotency
- Use a distinct `cart_id` for the refund request; do not blindly retry — re-query first to avoid a double refund. See `conventions/paytabs-conventions.yml`.

## Errors
- `206` currency mismatch on follow-up; `400` missing `tran_ref`; `401` auth/region. See `errors/paytabs-problem-types.yml` and `errors/paytabs-decline-codes.yml`.
