<!-- START_METADATA
---
title: Getting started
id: getting-started
sidebar_position: 10
toc_min_heading_level: 2
toc_max_heading_level: 5
pagination_next: null
pagination_prev: null
---

import ApiSchema from '@theme/ApiSchema';

END_METADATA -->

# Getting Started with the Vipps ePayment API

## Before you begin

This document covers the quick steps for getting started with the Vipps ePayment API.
You must have already signed up as a organisation with Vipps and have your test credentials from the merchant portal, as described in the
[Vipps Getting Started guide](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/vipps-getting-started).

<!-- START_COMMENT -->
<!--
Once your merchant account is setup for Merchant Payments, you should look at available configuration options, such as
[Notifications Webhooks](how-to-setup-notification-webhooks.md).
-->
<!-- END_COMMENT -->

## Your first Vipps Payment

This document targets the Test environment in Vipps which uses the domain `https://apitest.vipps.no`.

Once your account is active in production use the domain `https://api.vipps.no`.

### Step 1 - Authentication

```bash
curl https://apitest.vipps.no/accessToken/get \
-H "client_id: YOUR-CLIENT-ID" \
-H "client_secret: YOUR-CLIENT-SECRET" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-X POST
```

In response, you will get a body with the following schema.
The property `access_token` should be used for all other API requests in the `Authorization` header as the Bearer token.

<ApiSchema id="access-token-swagger-id" pointer="#/components/schemas/AuthorizationTokenResponse" example />

### Step 2 - Create a payment

To create a payment session, you need to send the specifications of that payment to Vipps. There is an extensive selection of options available which you can combine to make your custom payment experience.

To create a payment of 10 Norwegian Kroner, send a request like the one below:

```bash
curl https://apitest.vipps.no/epayment/v1/payments \
-H "Authorization: Bearer <TOKEN>" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-H "Content-Type: application/json" \
-H "Idempotency-Key: UNIQUE-ID" \
-H "Merchant-Serial-Number: YOUR-MERCHANT-ACCOUNT-NUMBER" \
-X POST \
-d '{
  "amount": {
    "currency": "NOK",
    "value": 1000
  },
  "paymentMethod": {
    "type": "WALLET"
  },
  "reference": "UNIQUE-PAYMENT-REFERENCE",
  "returnUrl": "https://yourwebsite.come/redirect?reference=abcc123",
  "userFlow": "NATIVE_REDIRECT",
  "paymentDescription": "A simple payment"
}'
```

The full list of payment options are:

<ApiSchema id="epayment-swagger-id" pointer="#/components/schemas/CreatePaymentRequest" />



A valid request like the one above will result in a response with the following structure.


<ApiSchema id="epayment-swagger-id" pointer="#/components/schemas/CreatePaymentResponse" example />


The `redirectUrl` property should be used to direct the user to the Vipps app for completing the payment.

### Step 3 - Completing the payment

The user will be presented with the payment in the Vipps app, where they can complete or reject the payment. Once the user has acted upon the payment they will be redirected back to the specified `returnUrl` under a "best effort" policy.

:::note
We cannot guarantee the user will be redirected back to the same browser or session, or that they will at all be redirected back. User interaction can be unpredictable and the user may choose to fully close the Vipps app or browser.
:::

To receive the result of the users action you may poll the status of the payment view the
[Get Payment][get-payment-endpoint] and
[Get Payment Event Log][get-payment-event-log-endpoint] endpoints.

<!-- START_COMMENT -->
<!--
Alternatively, receive status updates over our [Notification Webhooks](how-to-setup-notification-webhooks.md) service

-->
<!-- END_COMMENT -->

### Polling

A request to the [Get Payment][get-payment-endpoint] URL will provide the current status of the payment and an aggregate of the captured and refunded amounts.

Example request:

```bash
curl https://apitest.vipps.no/epayment/v1/payments/UNIQUE-PAYMENT-REFERENCE \
-H "Authorization: Bearer <TOKEN>" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-X GET
```

Response:

<ApiSchema id="epayment-swagger-id" pointer="#/components/schemas/GetPaymentResponse" example />

To verify that a payment has been authorized by the user, check that the `state` property is marked `AUTHORIZED`. If the user has instead chosen to reject the payment or chosen to click `cancel` on the landing page or in the Vipps App, the `state` property will be marked `ABORTED`. If the user did not act within the payment expiration time, the `state` property will be marked `EXPIRED`.

For more details of the lifecycle of the payment session the Event Log endpoint can be used

```bash
curl https://apitest.vipps.no/epayment/v1/payments/UNIQUE-PAYMENT-REFERENCE/events \
-H "Authorization: Bearer <TOKEN>" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-X GET
```

Response is a list of

<ApiSchema id="epayment-swagger-id" pointer="#/components/schemas/PaymentEvent" example />

In the case the payment has been completed this will yield an array of events like such:

```json
[
  {
    "reference": "UNIQUE-PAYMENT-REFERENCE",
    "pspReference": "497f6eca-6276-4993-bfeb-53cbbbba6f08",
    "name": "CREATED",
    "success": true,
    "amount": {
      "currency": "NOK",
      "value": 1000
    },
    "timestamp": "2021-02-24T14:15:22Z",
    "idempotencyKey": "IDEMPOTENCY-KEY-OF-REQUEST"
  },
  {
    "reference": "UNIQUE-PAYMENT-REFERENCE",
    "pspReference": "6037fc94-0205-4274-8b09-79cbb459658d",
    "name": "AUTHORIZED",
    "success": true,
    "amount": {
      "currency": "NOK",
      "value": 1000
    },
    "timestamp": "2021-02-24T14:16:22Z"
  }
]
```
<!-- START_COMMENT -->
<!--
### Notification Events

If you are not dependent on getting the payment result immediately, you may also use notification events to receive the payment status update via our [Notification Webhooks](how-to-setup-notification-webhooks.md) service. While we aim to deliver these event updates within a few seconds of the user completing the payment, this service has an eventual delivery guarantee rather than immediate delivery.

:::info
This means we may deliver the same message several times to verify successful delivery, use the `pspReference` field for duplicate delivery checking.
:::

If you use the notification service, you will receive events in the same format as those in the array list returned from the [Get Payment Events][get-payment-event-log-endpoint] endpoint.

For example, a successful authentication event would look like

```json
{
 "reference": "UNIQUE-PAYMENT-REFERENCE",
 "pspReference": "497f6eca-6276-4993-bfeb-53cbbbba6f08",
 "name": "CREATED",
 "success": true,
 "amount": {
   "currency": "NOK",
   "value": 1000
 },
 "timestamp": "2021-02-24T14:15:22Z",
 "idempotencyKey": "IDEMPOTENCY-KEY-OF-REQUEST"
}
```

If the user had rejected the payment, the event would look like

```json
{
  "reference": "UNIQUE-PAYMENT-REFERENCE",
  "pspReference": "38ab3a93-a819-4982-912d-089f3177e6c8",
  "name": "TERMINATED",
  "success": true,
  "amount": {
    "currency": "NOK",
    "value": 1000
  },
  "timestamp": "2021-02-24T14:15:12Z"
}
```
-->
<!-- END_COMMENT -->

## Next Steps

Now that you have completed your first payment,
read further to see the full range of possibilities within the Vipps ePayment API.

- [Capture the payment](modifications/capture.md)
- [Payment modification, how to use cancel, capture and refund?](modifications/README.md)
- [Using Vipps ePayment API in a shopper present context](features/customer-present-payments.md)
- [Profile sharing, requesting the users personal information](features/profile-sharing.md)

<!-- START_COMMENT -->
<!-- [How to setup Notification Webhooks](how-to-setup-notification-webhooks.md) -->
<!-- END_COMMENT -->

[create-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/CreatePayments/operation/createPayment
[get-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/QueryPayments/operation/getPayment
[get-payment-event-log-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/QueryPayments/operation/getPaymentEventLog
[cancel-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/cancelPayment
[capture-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/capturePayment
[refund-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/refundPayment
[adjust-authorization-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/adjustAuthorization
[force-approve-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/ForceApprove/operation/forceApprove
