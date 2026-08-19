---
name: emotive-connect-a-custom-helpdesk
description: Wire a custom helpdesk to Emotive — mint a brand token, register webhooks, and exchange ticket events.
api: Emotive Helpdesk API
spec: openapi/emotive-helpdesk-openapi.yml
base_url: https://api-gw.emotiveapp.co/helpdesk
operations:
- controllers.ticket_systems.get
- controllers.connections.post
- controllers.connections.get_brand_connections
- controllers.webhooks.post
- controllers.webhooks.get
- controllers.webhooks.delete
- controllers.tickets.update
- controllers.ticket_events.post_external
- controllers.tickets.webhook_trigger_data
generated: '2026-08-13'
method: generated
source: https://help.emotive.io/docs/integrations/custom-helpdesk
---

# Connect a custom helpdesk to Emotive

Use this when a brand runs a helpdesk Emotive does not integrate natively (its built-ins are
Zendesk, Gorgias, Kustomer, Zoho Desk and Freshdesk) and wants Emotive SMS conversations to
appear as tickets.

## 1. Mint a brand token

1. `controllers.ticket_systems.get` — `GET /ticket_systems` with
   `Authorization: bearer {emotive JWT}`. Find the `id` of your ticket-system type.
2. `controllers.connections.post` — `POST /connections/config` with the same JWT:

   ```json
   { "auth_type": "api_token", "status": "active", "ticket_system_type_id": 0 }
   ```

   Substitute the id from step 1. A brand may hold **one** active token at a time.
3. Every subsequent call uses `X-Api-Key: {brand_token}` instead of the JWT.

## 2. Register your two webhook endpoints

`controllers.webhooks.post` — `POST /webhooks/config` with `X-Api-Key: {brand_token}`:

```json
{ "status": "active", "platform": "custom",
  "webhook_url": "https://yoursite.com/api/ticket/created", "trigger": "create" }
```

Register a second one for ticket updates. Emotive calls the `create` endpoint when a ticket
is opened and the update endpoint on every subsequent subscriber message — after the first
ticket exists, all further events arrive as ticket **notes**.

If you cannot receive inbound HTTP, `controllers.tickets.webhook_trigger_data` —
`GET /tickets/polling/fallback` — is the published polling alternative.

## 3. Exchange ticket events

- **Acknowledge a new ticket:** `controllers.tickets.update` —
  `POST /tickets/{helpdesk_ticket_id}` with `{"external_ticket_id": "...", "external_user_id": "..."}`.
- **Reply to the subscriber:** `POST /tickets/{helpdesk_ticket_id}/events` with
  `{"message": "...", "status": "open", "event_time": "...", "system_type": "custom"}`.
- **Close the conversation:** `controllers.ticket_events.post_external` —
  `POST /tickets/external/{external_ticket_id}/events` with `{"status": "closed", ...}`.
- **Unsubscribe from an agent reply:** send `#optout` or `#opt-out` as the message body. It
  is not delivered to the subscriber; it opts them out of the platform.

## Rules that are easy to get wrong

- An SMS subscriber can have only **one active ticket at a time**. Do not open a second.
- Emotive signs nothing. There is no signature header, shared secret or replay window on
  the callbacks it makes to your endpoint — authenticate them yourself (mTLS, a secret in
  the path, or an allowlist) and assume replays are possible.
- The token lives in `X-Api-Key` here, not `Authorization`. Both header styles are in use
  across Emotive's surface.

## References

- asyncapi/emotive-webhooks.yml
- authentication/emotive-authentication.yml
