---
name: emotive-track-an-order
description: Send a completed order from a custom eCommerce site to Emotive so it can be attributed to an SMS subscriber.
api: Emotive Open API
spec: openapi/emotive-open-api-openapi.yml
base_url: https://api.emotiveapp.co
operations:
- createOrder
generated: '2026-08-13'
method: generated
source: https://help.emotive.io/docs/integrations/open-api-integration-orders
---

# Track an order in Emotive

Use this when a custom (non-Shopify, non-WooCommerce, non-BigCommerce) storefront needs
to report a completed order to Emotive for attribution and cart-recovery measurement.

## Prerequisites

- An Emotive Open API token. There is no self-service issuance — email `support@emotive.io`.
- The buyer's phone number in E.164 form. This is the only join key Emotive has between an
  order and a subscriber.

## Steps

1. Build the order body. `customer.phone`, `line_items[].product.title`,
   `line_items[].product.sku`, `line_items[].price`, `total_price`, `order_id` and
   `order_date` are the minimum Emotive documents as required. Everything else —
   `shipping_address`, `discount_codes`, `currency`, `customer_tags`, `extra_data` — is
   optional and is carried through.
2. Call `createOrder`: `POST /ecommerce/v1/orders/` with
   `Authorization: Token <api_token>` and `Content-Type: application/json; charset=UTF-8`.
3. Treat 200, 201 **and 292** as success. 292 is Emotive's own non-standard code meaning
   "accepted and queued for execution" — switch on the 2xx class, not on an exact code.
4. On 400, read the `error` attribute in the body; it names the missing or invalid field.

## Rules that are easy to get wrong

- The `Authorization` value is the literal word `Token`, a space, then the key. A bare key
  returns 401 `{"detail":"Authentication credentials were not provided."}`.
- **There is no idempotency key.** Emotive publishes none, and this operation creates an
  order record. If a request times out you cannot safely retry it — you will double-count
  the order and may re-trigger an attribution flow. Record `order_id` locally with the
  outcome and reconcile before resending.
- There is no rate limit published and no `Retry-After` header. Pace conservatively.
- `api.emotiveapp.co` soft-404s: an unknown path returns HTTP 200 with an HTML app shell,
  not a 404. Always assert `Content-Type: application/json` before parsing a response.

## References

- errors/emotive-problem-types.yml
- conventions/emotive-conventions.yml
- rate-limits/emotive-rate-limits.yml
