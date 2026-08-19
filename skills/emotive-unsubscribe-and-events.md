---
name: emotive-unsubscribe-and-events
description: Opt a phone number out of Emotive SMS, and push a custom event that triggers an Emotive Flow.
api: Emotive Open API
spec: openapi/emotive-open-api-openapi.yml
base_url: https://api.emotiveapp.co
operations:
- manualOptOut
- createCustomEvent
generated: '2026-08-13'
method: generated
source: https://emotive.gitbook.io/emotive-lists/reference/api-reference/unsubscribe
---

# Unsubscribe a number, and trigger a Flow from your own event

Two small, high-consequence operations on the Emotive Open API.

## Unsubscribe a phone number

`manualOptOut` — `POST /ecommerce/v1/opt_outs/manual_opt_out/`

```
Content-Type: application/json
Authorization: Token <YOUR_API_KEY>

{ "phone_number": "+14155552671" }
```

- `200` → `{"message": "Opt out request processed."}`
- `400` → `{"message": "Invalid phone number."}`

Phone numbers must be E.164. This is the programmatic equivalent of a consumer replying
STOP; treat it as compliance-critical and log every call. Emotive also honours an inbound
`#optout` or `#opt-out` message on the helpdesk path, which unsubscribes the user without
delivering the message to the brand.

## Push a custom event

`createCustomEvent` — `POST /campaign_engine/api/v1/custom_events/`

```
Content-Type: application/json
Authorization: Token <YOUR_API_KEY>

{
  "phone_number": "+14155552671",
  "event_name": "user-clicked",
  "event_datetime": "2026-08-13T10:10:10",
  "properties": { "plan": "pro" }
}
```

- Identify the subscriber with `phone_number` **or** `email` — at least one is required.
- `event_name` and `event_datetime` are both required; omitting them returns
  `400 {"event_name":["This field is required."],"event_datetime":["This field is required."]}`.
- `event_datetime` is ISO-8601.
- `200` → `{"message": "success"}`. `401` → `{"detail": "Invalid token."}`.

The event does nothing on its own. To act on it, create a Flow (Experience) in the app with
a **Custom Event** trigger filtered to the same `event_name`.

## Rules that are easy to get wrong

- Note the error-envelope change across these two operations: the opt-out returns
  `{"message": ...}`, the custom event returns field-keyed arrays on 400 and
  `{"detail": ...}` on 401. Parse defensively; there is no single error shape.
- Neither operation is idempotent and neither is rate-limited in any published document.

## References

- errors/emotive-problem-types.yml
- conventions/emotive-conventions.yml
