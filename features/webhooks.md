---
title: Webhooks
sidebar_label: Webhooks
hide_table_of_contents: true
sidebar_position: 60
---

import ApiSchema from '@theme/ApiSchema';

# Webhooks

You can subscribe to webhooks for any number of events.
See the
[Webhooks API guide](https://developer.vippsmobilepay.com/docs/APIs/webhooks-api)
to get started.

For any webhook, you will receive the following payload. The only property changing is the `NAME`, which will reflect the event type you subscribed on.

<ApiSchema id="epayment-swagger-id" pointer="#/components/schemas/WebhookEvent" />

For the full list of available events, go to
[Webhooks events](https://developer.vippsmobilepay.com/docs/APIs/webhooks-api/events).
