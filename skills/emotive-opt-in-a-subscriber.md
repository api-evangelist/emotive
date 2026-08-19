---
name: emotive-opt-in-a-subscriber
description: Opt a customer in to Emotive SMS from an external system using the List Growth API, then set profile properties on them.
api: Emotive Lists API (Subscriber Engine) + Emotive Sensus Webhooks API
spec: openapi/emotive-subscriber-engine-openapi.yml, openapi/emotive-sensus-webhook-openapi.yml
base_url: https://www.emotiveapp.co, https://api-gw.emotiveapp.co
operations:
- get:/subscriber_engine/api/v1/sources/subscriber_lists/
- controllers.webhooks.subscriber_handler
- controllers.webhooks.bulk_subscriber_handler
- controllers.webhooks.profile_handler
generated: '2026-08-13'
method: generated
source: https://emotive.gitbook.io/emotive-lists/integration-setup-instructions
---

# Opt a subscriber in to Emotive SMS

Use this when an external system (a CRM, a custom signup form, a partner platform) needs
to push new SMS or email subscribers into Emotive.

## Prerequisites

Create a List Growth API signup flow first — the API key does not exist until you do.
In the app: **List Growth → Integrations → Add New → List Growth API**, name the flow,
select or create an Experience, and save the key it issues at the end. The flow you create
is the list that will appear in the subscriber-lists endpoint.

## Steps

1. **Discover the target list.** `GET https://www.emotiveapp.co/subscriber_engine/api/v1/sources/subscriber_lists/`
   with `Authorization: <YOUR_API_KEY>`. It returns `[{subscribe_identifier, name}]` — the
   active signup flows. Pick the `subscribe_identifier` for the flow you created.
2. **Create the subscriber.** `controllers.webhooks.subscriber_handler` —
   `POST https://api-gw.emotiveapp.co/sensus-webhook/v1/subscriber` with
   `Authorization: Token <YOUR_API_KEY>`. Send at minimum `phone_number` (or `email`),
   `source_type: LIST_GROWTH`, and the `subscribe_identifier` from step 1.
   Do **not** send `platform`, `popup_identifier` or `subsource_identifier` — Emotive's docs
   state those must be omitted on this path.
3. Expect `202` (Webhook Accepted), not `200`. `400` means the body failed validation.
4. **For a batch,** use `controllers.webhooks.bulk_subscriber_handler` —
   `POST /v1/subscriber/bulk` — rather than looping the single-subscriber call.
5. **Attach traits.** `controllers.webhooks.profile_handler` —
   `POST /v1/profiles/properties` — takes `source`, `platform` and a `properties[]` array of
   `{key, value, type}` triples, where `type` is one of `string`, `number`, `boolean`,
   `datetime`. `platform` must be one of `alloy`, `paragon`, `custom`, `zapier`.

## Rules that are easy to get wrong

- Note the header inconsistency: the subscriber-lists endpoint is documented with a bare
  key (`Authorization: <YOUR_API_KEY>`), while every other endpoint takes the `Token `
  prefix. Follow each page exactly.
- The two calls live on **different hosts** — `www.emotiveapp.co` for list discovery,
  `api-gw.emotiveapp.co` for the write.
- **Compliance is not optional.** Every SMS opt-in triggers a TCPA-compliant follow-up
  message with unsubscribe instructions. You must have prior express written consent before
  pushing a number. See conformance/emotive-conformance.yml.
- No idempotency key exists. Re-sending the same subscriber is not a documented no-op.

## References

- authentication/emotive-authentication.yml
- conformance/emotive-conformance.yml
- https://emotive.gitbook.io/emotive-lists/reference/authentication
