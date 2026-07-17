---
generated: '2026-07-17'
method: generated
name: Accept a hosted-page payment
description: Create a PayTabs hosted payment page, redirect the buyer, then confirm the outcome.
api: openapi/paytabs-openapi.yml
operations: [createPaymentRequest, queryTransaction]
source: >-
  operationIds verified in openapi/paytabs-openapi.yml. Conventions/errors cite
  conventions/paytabs-conventions.yml and errors/paytabs-decline-codes.yml.
---

# Accept a hosted-page payment

Collect a card payment through the PayTabs-hosted page (lowest PCI scope).

## Auth
- Send the merchant **server key** as the raw value of the `authorization` header (not Bearer). See `authentication/paytabs-authentication.yml`.
- Use the region host that matches the profile (e.g. `secure.paytabs.sa` for KSA) or you get a 401.

## Idempotency
- Set a unique `cart_id`. PayTabs rejects an identical request for the same `cart_id` within ~2 minutes (response code 4), preventing double charges. See `conventions/paytabs-conventions.yml`.

## Steps
1. **Create the transaction** — `createPaymentRequest` (`POST /payment/request`) with `profile_id`, `tran_type: sale`, `tran_class: ecom`, `cart_id`, `cart_description`, `cart_currency`, `cart_amount`, a `callback` (IPN) and `return` URL. Read `redirect_url` from the response and send the buyer there.
2. **Receive the IPN/callback** — PayTabs POSTs the terminal result to your `callback` URL with a `Signature` header (HMAC SHA-256 of the body keyed by the ServerKey). Verify the signature before trusting it. See `asyncapi/paytabs-webhooks-asyncapi.yml`.
3. **Confirm** — `queryTransaction` (`POST /payment/query`) with `profile_id` and the `tran_ref` (or `cart_id`) to read the authoritative state. Treat `payment_result.response_status = A` as authorized.

## Errors
- `400` missing/invalid fields; `401` bad key or wrong region host. In-body declines carry `payment_result.response_code` — map via `errors/paytabs-decline-codes.yml` (e.g. 316 insufficient funds, 310 3DS rejected). Never show the raw decline reason to the buyer.

## Testing
- Use a test profile and dashboard test cards — see `sandbox/paytabs-sandbox.yml`.
