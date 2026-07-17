---
generated: '2026-07-17'
method: generated
name: Tokenize a card and charge it recurringly
description: Save a card as a PayTabs token, then charge the token for unattended recurring billing.
api: openapi/paytabs-openapi.yml
operations: [createPaymentRequest]
source: >-
  operationId verified in openapi/paytabs-openapi.yml. Tokenization and recurring
  charges reuse POST /payment/request with tokenise/register and tran_class=recurring.
---

# Tokenize a card and charge it recurringly

## Auth
- Merchant **server key** in the `authorization` header on the region host. The initial card capture may use the browser **client key** (own-form). See `authentication/paytabs-authentication.yml`.

## Steps
1. **Capture and tokenize** — `createPaymentRequest` (`POST /payment/request`) with `tran_type: sale` (or `register` for a zero/verify), `tran_class: ecom`, and `tokenise` set (1-6) plus `show_save_card` if buyer-consented. Read the returned `token` from the response.
2. **Store the token** — persist `token` against your customer. Never store the PAN; PayTabs holds the card in PCI scope.
3. **Charge recurringly** — `createPaymentRequest` with `tran_class: recurring`, `tran_type: sale`, the saved `token`, a new unique `cart_id`, and `cart_amount`/`cart_currency`. No buyer interaction is needed for the unattended charge.

## Idempotency
- Each recurring charge needs its own unique `cart_id` (duplicate within ~2 minutes → response code 4). See `conventions/paytabs-conventions.yml`.

## Errors
- Declines on a recurring charge (e.g. 316 insufficient funds, 300 not authorised) arrive in `payment_result.response_code` — see `errors/paytabs-decline-codes.yml`. Implement dunning/retry with backoff; do not hammer a declining token.

## Consequence
- Unattended charges move money — gate with approval/policy per `agentic-access/paytabs-agentic-access.yml`.
