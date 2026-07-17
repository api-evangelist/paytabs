# PayTabs (paytabs)

PayTabs is a Saudi-built payment orchestration and gateway provider serving merchants across MENA. The PT2 REST API accepts cards and local methods (mada, Meeza, KNET, OmanNet, Benefit, STC Pay, urpay) plus Apple Pay, Google Pay and Samsung Pay through hosted, managed and own-form flows, with tokenization, recurring billing and invoicing. The API is served on region-specific hosts and authenticated with a merchant server key.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/paytabs/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/paytabs/refs/heads/main/apis.yml)

## Tags

- Payments
- Payment Gateway
- Fintech
- MENA
- Saudi Arabia
- Cards
- mada

## Timestamps

- **Created:** 2026-07-17
- **Modified:** 2026-07-17

## Region Hosts

PayTabs serves the same PT2 endpoints on region-specific hosts. Use the host that matches your profile's region or authentication will fail.

| Region | Host |
| --- | --- |
| UAE / default | `https://secure.paytabs.com` |
| Saudi Arabia (KSA) | `https://secure.paytabs.sa` |
| Egypt | `https://secure-egypt.paytabs.com` |
| Oman | `https://secure-oman.paytabs.com` |
| Jordan | `https://secure-jordan.paytabs.com` |
| Kuwait | `https://secure-kuwait.paytabs.com` |
| Iraq | `https://secure-iraq.paytabs.com` |
| Morocco | `https://secure-morocco.paytabs.com` |
| Qatar | `https://secure-doha.paytabs.com` |
| Global | `https://secure-global.paytabs.com` |

## Authentication

Server-to-server calls send the merchant **server key** as the raw value of the `authorization` HTTP header (not a Bearer token), with `profile_id` in the request body. A separate **client key** is used for browser-side own-form and card tokenization. Keys are obtained from the merchant dashboard under Developers > Key management and are region-scoped.

## APIs

### PayTabs Hosted Payment Page API

Creates a transaction via POST /payment/request and returns a hosted redirect_url where the customer completes payment on PayTabs-hosted, PCI-scope-reducing pages. Supports cards, mada, Apple/Google/Samsung Pay and local methods, with callback (IPN) and return URLs.

- **Human URL:** [https://docs.paytabs.com/manuals/PT-API-Endpoints/Integration-Types-Manuals/Hosted-Payment-Page/HPP-Step-3-Initiating-Payment/HPP-Initiating-Payment/](https://docs.paytabs.com/manuals/PT-API-Endpoints/Integration-Types-Manuals/Hosted-Payment-Page/HPP-Step-3-Initiating-Payment/HPP-Initiating-Payment/)
- **Base URL:** `https://secure.paytabs.com`

#### Tags

- Payments
- Hosted Payment Page
- Redirect

#### Properties

- [Documentation](https://docs.paytabs.com/manuals/PT-API-Endpoints/Introduction/)
- [API Reference](https://documenter.getpostman.com/view/14575178/TWDRtfWG)
- [OpenAPI](openapi/paytabs-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/paytabs.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)

### PayTabs Managed Form API

Same POST /payment/request flow rendered inside a merchant-page iframe (framed=true) so the checkout is embedded while card capture stays in PayTabs' PCI scope.

- **Human URL:** [https://docs.paytabs.com/PT2-API-Endpoints/Integration-Types-Manuals/Hosted-Payment-Page/HPP-Step-2-Configure-Integration-Method/HPP-Step-2-Landing/](https://docs.paytabs.com/PT2-API-Endpoints/Integration-Types-Manuals/Hosted-Payment-Page/HPP-Step-2-Configure-Integration-Method/HPP-Step-2-Landing/)
- **Base URL:** `https://secure.paytabs.com`

#### Tags

- Payments
- Managed Form
- iFrame

#### Properties

- [Documentation](https://docs.paytabs.com/manuals/PT-API-Endpoints/Introduction/)
- [OpenAPI](openapi/paytabs-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/paytabs.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)

### PayTabs Own Form API

Merchant-branded checkout that collects card data on the merchant page (client key) and submits to POST /payment/request, requiring a higher PCI SAQ level than the hosted flows.

- **Human URL:** [https://docs.paytabs.com/manuals/PT-API-Endpoints/Integration-Types-Manuals/Own-Form/Own-Form-Step-5-handle-the-payment-response/Own-Form-Step-5-Landing/](https://docs.paytabs.com/manuals/PT-API-Endpoints/Integration-Types-Manuals/Own-Form/Own-Form-Step-5-handle-the-payment-response/Own-Form-Step-5-Landing/)
- **Base URL:** `https://secure.paytabs.com`

#### Tags

- Payments
- Own Form
- Tokenization

#### Properties

- [Documentation](https://docs.paytabs.com/manuals/PT-API-Endpoints/Introduction/)
- [OpenAPI](openapi/paytabs-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/paytabs.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)

### PayTabs Transaction Management API

Reads transaction state via POST /payment/query (by tran_ref or cart_id) and performs follow-up actions - capture, void, release and refund - via POST /payment/request with the matching tran_type.

- **Human URL:** [https://docs.paytabs.com/manuals/PT-API-Endpoints/Integration-Types-Manuals/Hosted-Payment-Page/HPP-Step-7-Manage-Transactions/HPP-Step-7-Query-Transaction/](https://docs.paytabs.com/manuals/PT-API-Endpoints/Integration-Types-Manuals/Hosted-Payment-Page/HPP-Step-7-Manage-Transactions/HPP-Step-7-Query-Transaction/)
- **Base URL:** `https://secure.paytabs.com`

#### Tags

- Transactions
- Query
- Refund
- Capture

#### Properties

- [Documentation](https://docs.paytabs.com/manuals/PT-API-Endpoints/Integration-Types-Manuals/Hosted-Payment-Page/HPP-Step-7-Manage-Transactions/HPP-Step-7-Query-Transaction/)
- [OpenAPI](openapi/paytabs-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/paytabs.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)

### PayTabs Token & Recurring Billing API

Saves a card token (tokenise/register) then charges it for unattended recurring or repeat-billing payments via POST /payment/request with tran_class=recurring and the stored token.

- **Human URL:** [https://docs.paytabs.com/manuals/PT-API-Endpoints/Repeat-Billing/Step-1-Understanding-Workflow-&-Prerequisites/Repeat-Billing-Prerequisites/](https://docs.paytabs.com/manuals/PT-API-Endpoints/Repeat-Billing/Step-1-Understanding-Workflow-&-Prerequisites/Repeat-Billing-Prerequisites/)
- **Base URL:** `https://secure.paytabs.com`

#### Tags

- Tokenization
- Recurring
- Repeat Billing

#### Properties

- [Documentation](https://docs.paytabs.com/manuals/PT-API-Endpoints/Repeat-Billing/Step-1-Understanding-Workflow-&-Prerequisites/Repeat-Billing-Prerequisites/)
- [OpenAPI](openapi/paytabs-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/paytabs.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)

### PayTabs Invoices API

Generates payable invoices / payment links over POST /payment/request with invoice fields, deliverable to customers and downloadable as PDF or sent via SMS.

- **Human URL:** [https://support.paytabs.com/en/support/solutions/articles/60000902481-3-2-3-invoices-apis-initiating-the-payment-request-download-invoice-pdf](https://support.paytabs.com/en/support/solutions/articles/60000902481-3-2-3-invoices-apis-initiating-the-payment-request-download-invoice-pdf)
- **Base URL:** `https://secure.paytabs.com`

#### Tags

- Invoices
- Payment Links

#### Properties

- [Documentation](https://support.paytabs.com/en/support/solutions/articles/60000902481-3-2-3-invoices-apis-initiating-the-payment-request-download-invoice-pdf)
- [OpenAPI](openapi/paytabs-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/paytabs.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)

## Common Properties

- [GitHub Organization](https://github.com/paytabscom)
- [LinkedIn](https://www.linkedin.com/company/paytabs)
- [Website](https://paytabs.com/en/)
- [Documentation](https://docs.paytabs.com/)
- [Plans](plans/paytabs-plans-pricing.yml)
- [Rate Limits](rate-limits/paytabs-rate-limits.yml)
- [Fin Ops](finops/paytabs-finops.yml)

## Notes

- **Company:** PayTabs Group, founded 2014 in Saudi Arabia (founder/CEO Abdulaziz Fahad Al Jouf), with offices in KSA, UAE and Egypt and presence across MENA, Turkey and India.
- **Compliance:** PCI DSS Level 1; EMV 3-D Secure 2 via Modirum (first deployed in the Kingdom of Saudi Arabia); certified for the Saudi national scheme (mada).
- **Async surface:** post-payment updates arrive as an HTTP IPN webhook to the merchant `callback` URL plus a browser `return` redirect. No public WebSocket API (see [review.yml](review.yml)).
- **Spec provenance:** the OpenAPI is modeled from the PayTabs PT2 documentation and public Postman reference (no first-party OpenAPI is published).

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
