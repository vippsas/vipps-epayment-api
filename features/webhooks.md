---
title: Webhooks
sidebar_label: Webhooks
hide_table_of_contents: true
sidebar_position: 60
---

import ApiSchema from '@theme/ApiSchema';

# Webhooks

The ePayment API offers the possibility to subscribe to webhooks for any number of events:
* You register a webhook URL
* We send updates and information to the webhook URL you have specified

See the
[Webhooks API](https://developer.vippsmobilepay.com/docs/APIs/webhooks-api)
for more information.

For any webhook, you will receive the following payload, where the only property changing is
the `name`, which will reflect the event type you subscribed on.

<ApiSchema id="epayment-swagger-id" pointer="#/components/schemas/WebhookEvent" />

For the full list of available events, see:
[Webhooks events](https://developer.vippsmobilepay.com/docs/APIs/webhooks-api/events).
