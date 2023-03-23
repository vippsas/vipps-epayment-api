<!-- START_METADATA
---
title: Webhooks
sidebar_label: Webhooks
hide_table_of_contents: true
pagination_next: null
pagination_prev: APIs/epayment-api/quick-start
sidebar_position: 60
---

import ApiSchema from '@theme/ApiSchema';

END_METADATA -->

# Webhooks

💥 Work in progress 💥


<!-- START_COMMENT -->
<!--
Add some nice text about getting started with Webhooks
[Notifications Webhooks](how-to-setup-notification-webhooks.md).
-->
<!-- END_COMMENT -->


ePayments API offers the possibility to subscribe to webhooks for any number of events.
For any webhook you will receive the following payload, where the only property changing is the `NAME`, which will reflect the event type you subscribed on.

<ApiSchema id="epayment-swagger-id" pointer="#/components/schemas/PaymentEvent" />

<!-- START_COMMENT -->
<!--
For the full list of available events, go to
[Notifications Webhooks](how-to-setup-notification-webhooks.md).
-->
<!-- END_COMMENT -->
