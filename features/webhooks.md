---
title: Webhooks
sidebar_label: Webhooks
hide_table_of_contents: true
sidebar_position: 60
---

import ApiSchema from '@theme/ApiSchema';


# Webhooks

:::note
Webhooks will be released early Q2.
:::



ePayment API offers the possibility to subscribe to webhooks for any number of events.
Visit the
[Webhooks API](https://developer.vippsmobilepay.com/docs/APIs/webhooks-api)
for general information about webhooks, and how to get started.
For any webhook you will receive the following payload, where the only property changing is the `NAME`, which will reflect the event type you subscribed on.

<ApiSchema id="epayment-swagger-id" pointer="#/components/schemas/WebhookEvent" />

For the full list of available events, go to
[Webhooks events](https://developer.vippsmobilepay.com/docs/APIs/webhooks-api/events).