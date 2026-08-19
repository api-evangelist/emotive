---
name: Emotive
description: Use when building SMS marketing campaigns, setting up automated flows, managing customer conversations, creating audience segments, or integrating SMS with ecommerce platforms. Reach for this skill when users need to send broadcasts, set up list growth, manage compliance, or analyze SMS campaign performance.
metadata:
    mintlify-proj: emotive
    version: "1.0"
---

# Emotive Skill Reference

## Product summary

Emotive is an SMS marketing platform for ecommerce brands. Agents use it to send bulk text messages (broadcasts), build automated workflows (flows), manage customer conversations, segment audiences, and integrate SMS with Shopify, WooCommerce, Klaviyo, and other platforms. The primary dashboard is at https://www.emotiveapp.co. Key workflows include creating broadcasts, building flows with triggers, defining segments, and managing customer responses. Compliance with TCPA regulations is mandatory before sending any messages.

## When to use

Reach for this skill when:
- A user wants to send a bulk SMS message to their customer list (broadcast)
- A user needs to set up automated SMS sequences triggered by customer actions (flows)
- A user wants to grow their SMS list via popups, forms, or integrations
- A user needs to segment their audience for targeted messaging
- A user is managing customer replies to SMS messages (conversations)
- A user needs to integrate SMS with their ecommerce platform (Shopify, WooCommerce, etc.)
- A user is setting up compliance for SMS marketing (TCPA, CCPA, opt-in language)
- A user needs to analyze SMS campaign performance and ROI
- A user is troubleshooting delivery issues or spam folder problems

## Quick reference

### Core workflows

| Task | Location | Key steps |
|------|----------|-----------|
| Send a broadcast | Broadcasts → Create Broadcast | Name broadcast, select segments, write message, add image/discount, schedule or send |
| Build a flow | Flows → New Experience | Choose trigger (order, abandoned cart, opt-in, etc.), set schedule, build message sequence, add conditional splits |
| Create a segment | Segments → Create New Segment | Name segment, add rules (shopping activity, engagement, location, properties), use AND/OR logic |
| Manage conversations | Conversations queue | Search by name/phone/content, reply to customers, mark read/unread, close or opt-out |
| Upload subscriber list | Get Started → Upload List | Submit form certifying TCPA-compliant collection, wait for approval |
| Set up integration | Integrations → [Platform] | Follow platform-specific setup (API keys, webhooks, form embeds) |

### Broadcast best practices

- **Frequency**: 2–6 broadcasts per month
- **Timing**: Wednesday/Saturday, 9am PST / 12pm EST
- **Image**: Include for large sends (600 KB max, JPEG/PNG)
- **Length**: Max 1,000 characters; keep concise
- **Format**: Brand name first, then message body
- **Call-to-action**: "Tap to shop: [LINK]" or "Tap to learn more: [LINK]"
- **Offers**: Make SMS-exclusive to prevent opt-outs
- **Follow-ups**: Enable follow-up for non-purchasers (generates ~82% more sales)

### Flow triggers

| Trigger | Use case |
|---------|----------|
| Subscriber orders | Post-purchase flows, thank you messages |
| Abandoned checkout | Cart recovery (limit to 1 message within 48 hours) |
| Subscriber clicks link | Engagement-based sequences |
| Subscriber opts-in | Welcome flows |
| Subscriber enters Klaviyo list | Sync with email marketing |
| Order shipped/delivered | Tracking and delivery notifications (Shopify) |
| Subscription events | Recharge: signup, payment failure, upcoming order |

### Segment rules

**Engagement**: Text received, broadcast received, link clicked, reply sent  
**Shopping**: Abandoned checkout, order placed, subscription order, lifetime spend, customer tags  
**Properties**: State, timezone, signup method, Klaviyo list membership  
**Operators**: Equal, not equal, greater than, less than, set, not set

### Compliance requirements (TCPA)

- Obtain **prior expressed written consent** from each subscriber
- Consent must explicitly cover: (1) recurring marketing messages, (2) automatic dialing system use, (3) consent is not a condition of purchase
- Include TCPA-compliant opt-in language on all forms, popups, emails
- Do not pre-check opt-in boxes
- Respect quiet hours: 8am–9pm recipient's local timezone (stricter in WA, FL, OK)
- Abandoned cart: max 1 message within 48 hours
- Retain proof of consent for litigation defense

### Integrations by category

**Ecommerce**: Shopify, WooCommerce, BigCommerce, Privy, Recharge, Magento  
**Email/CRM**: Klaviyo, Mailchimp, Omnisend, ActiveCampaign, HubSpot, Drip, Customer.io  
**Loyalty**: Yotpo, Smile.io, Loyalty Lion  
**Helpdesk**: Zendesk, Gorgias, Freshdesk, Zoho Desk, Kustomer  
**Custom**: Zapier, custom API, custom webhooks, Profile Properties API

## Decision guidance

### When to use Broadcast vs Flow

| Scenario | Use Broadcast | Use Flow |
|----------|---------------|----------|
| One-time announcement | ✓ | |
| Triggered by customer action | | ✓ |
| Recurring schedule (weekly sale) | ✓ | |
| Multi-step sequence | | ✓ |
| A/B testing copy | ✓ | ✓ |
| Personalized journey | | ✓ |
| Abandoned cart recovery | | ✓ |
| New product launch | ✓ | |

### When to use AND vs OR in segments

| Logic | Example | Result |
|-------|---------|--------|
| AND | Lifetime spend > $100 **AND** State = CA | Only CA customers who spent $100+ |
| OR | Placed order **OR** Abandoned checkout | Anyone who engaged with checkout |
| Mixed | (Placed order **OR** Abandoned checkout) **AND** Timezone = EST | Engaged users in EST timezone |

### Integration choice: Zapier vs native integration

| Scenario | Use Zapier | Use native |
|----------|-----------|-----------|
| Platform has native Emotive integration | | ✓ |
| Need to connect unsupported platform | ✓ | |
| Require real-time sync | | ✓ |
| Simple one-way data flow | ✓ | |
| Complex multi-step automation | ✓ | |

## Workflow

### Typical task: Send a broadcast to a segment

1. **Understand the goal**: What action do you want subscribers to take? (Shop, click link, reply?)
2. **Check existing segments**: Navigate to Segments and search for relevant audience (e.g., "High spenders", "Abandoned cart"). If it doesn't exist, create one.
3. **Create the broadcast**: Go to Broadcasts → Create Broadcast. Name it descriptively (e.g., "Flash Sale - Jan 2025").
4. **Select segments**: Choose 1+ segments to include. Optionally exclude segments (e.g., exclude recent purchasers).
5. **Write the message**: Start with brand name, keep under 1,000 characters, include clear CTA, add image if sending to large list.
6. **Add incentive** (optional): Include discount code or exclusive offer.
7. **Enable follow-up** (optional): Toggle "Follow up with customers who haven't ordered" and set delay (1–48 hours).
8. **Test**: Click "Send me a test" to preview on your phone.
9. **Review and send**: Click "Review and send", verify copy/segments, choose schedule or immediate send.
10. **Monitor performance**: Check Broadcast Timeline for UCTR, CVR, sales, ROI, unsubscribe rate.

### Typical task: Build an abandoned cart flow

1. **Navigate to Flows**: Click Flows → New Experience.
2. **Choose trigger**: Select "Subscriber abandons their checkout".
3. **Set schedule**: Configure send time (default 9am–5pm PT; adjust as needed).
4. **Build message 1**: Write first message (e.g., "You left something behind!"), add discount code, include link to cart.
5. **Add conditional split** (optional): Route based on segment (e.g., VIP vs new customer) or trigger event.
6. **Add follow-up message** (optional): Set delay (e.g., 24 hours) and write second message.
7. **Set exit rules**: Define when contacts leave the flow (e.g., after purchase, after 7 days).
8. **Test**: Send test message to yourself.
9. **Launch**: Activate the flow. It now runs automatically.
10. **Monitor**: Check flow analytics for conversion rate and revenue.

### Typical task: Set up TCPA-compliant opt-in

1. **Review TCPA requirements**: Read docs/compliance/sms-marketing-compliance-guide.
2. **Add opt-in language to forms**: Include this text on all signup forms, popups, emails:
   > "I agree to receive recurring automated marketing text messages (e.g. cart reminders) at the phone number provided. Consent is not a condition to purchase. Msg & data rates may apply. Msg frequency varies. Reply HELP for help and STOP to cancel. View our [Terms of Service](URL) and [Privacy Policy](URL)."
3. **Do not pre-check boxes**: Ensure opt-in checkboxes are unchecked by default.
4. **Hyperlink legal docs**: Link to your Terms of Service and Privacy Policy in the opt-in language.
5. **Retain consent records**: Keep proof of consent for each subscriber (required for litigation defense).
6. **Upload list**: If importing existing subscribers, submit form at docs.google.com/forms certifying TCPA-compliant collection.
7. **Verify quiet hours**: Set flow schedules to respect 8am–9pm recipient timezone (stricter in WA, FL, OK).

## Common gotchas

- **Pre-checked opt-in boxes**: Violates TCPA. Always leave boxes unchecked by default.
- **Missing hyperlinks in consent language**: Terms and Privacy Policy must be clickable links or full URLs must be visible.
- **Abandoned cart: multiple messages**: Carriers block non-compliant senders. Limit to 1 message within 48 hours.
- **Sending outside quiet hours**: Increases unsubscribes and spam complaints. Respect 8am–9pm recipient timezone.
- **No brand name in broadcast**: Always start with brand name (e.g., "-Emotive: Hi Sara..."). Improves deliverability.
- **Uploading non-compliant lists**: You certify consent when uploading. Non-compliant lists can result in account suspension.
- **Forgetting to add CTA**: Messages without clear action ("Tap to shop", "Reply YES") underperform.
- **Sending same offer on SMS and email**: Subscribers opt out if they see the same deal elsewhere. Make SMS offers exclusive.
- **Image too large**: MMS images must be under 5 MB. Resize before uploading.
- **Segment not refreshing**: Click the refresh icon to update contact count after adding new subscribers.
- **Flow not triggering**: Verify trigger is set correctly (e.g., "Subscriber orders" vs "Subscriber enters Klaviyo list"). Check that contacts meet the trigger condition.
- **Conversations not appearing**: Ensure customer replied to a message you sent. Inbound messages from non-subscribers won't appear.
- **Integration not syncing**: Verify API keys, webhooks, and permissions are correct. Check integration status in dashboard.
- **Blocked/restricted words**: Carriers block certain keywords (sex, hate, firearms, tobacco, CBD). Check docs/compliance/blocked-and-restricted-words.

## Verification checklist

Before launching a broadcast or flow:

- [ ] Segment is correctly defined and contact count is accurate (refresh if needed)
- [ ] Message includes brand name at the start
- [ ] Message has a clear call-to-action ("Tap to shop", "Reply YES", etc.)
- [ ] Message is under 1,000 characters
- [ ] Image is under 5 MB (if MMS)
- [ ] Discount code is valid and active (if included)
- [ ] Test message received on your phone without errors
- [ ] Opt-in language includes hyperlinked Terms and Privacy Policy (for new flows)
- [ ] Flow schedule respects quiet hours (8am–9pm recipient timezone)
- [ ] Abandoned cart flows limited to 1 message within 48 hours
- [ ] No blocked/restricted words in message
- [ ] Segment exclusions are correct (e.g., not sending to recent purchasers)
- [ ] Follow-up delay is set (1–48 hours for abandoned cart)
- [ ] Exit rules are configured (when contacts leave the flow)

## Resources

**Comprehensive navigation**: https://help.emotive.io/llms.txt

**Critical pages**:
- [SMS Marketing Compliance Guide](https://help.emotive.io/docs/compliance/sms-marketing-compliance-guide) — TCPA, CCPA, consent, quiet hours, blocked words
- [Create and Send a Broadcast](https://help.emotive.io/docs/broadcasting/create) — Step-by-step broadcast workflow
- [Understand Flow Triggers](https://help.emotive.io/docs/flows/triggers) — All available automation triggers
- [Create and Manage Segments](https://help.emotive.io/docs/segments/build-a-segment) — Segment rules and logic
- [Broadcast Best Practices](https://help.emotive.io/docs/best-practices/broadcasts) — Frequency, timing, copy guidelines
- [Integrations Overview](https://help.emotive.io/docs/integrations/overview) — All available platform integrations

---

> For additional documentation and navigation, see: https://help.emotive.io/llms.txt