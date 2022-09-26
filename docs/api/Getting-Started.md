<!-- START_METADATA
---
title: Getting started
sidebar_position: 100
---
END_METADATA -->

# Getting Started with the Vipps Merchant Payments API

💥 DRAFT! Unfinished work in progress. API specification changes are still coming. 💥

## Before you begin

This document covers the quick steps for getting started with the Vipps Merchant Payments API.
You must have already signed up as a organisation with Vipps and have your test credentials from the merchant portal, as described in the
[Getting Started guide](https://github.com/vippsas/vipps-developers/blob/master/vipps-getting-started.md#requirements).

Once your merchant account is setup for Merchant Payments, you should look at our [Configure Merchant Account](../TODO.md) page for available configuration options, such as our [Notifications Webhooks](How-to-setup-Notification-Webhooks.md).

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

In response you will get a body whith the following schema.
The property `access_token` should be used for all other API requests in the `Authorisation` header as the Bearer token.

```json
{
  "token_type": "Bearer",
  "expires_in": "3599",
  "ext_expires_in": "3599",
  "expires_on": "1614116654",
  "not_before": "1614112754",
  "resource": "00000002-0000-0000-c000-000000000000",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Im5PbzNaRHJPRFhFSzFqS1doWHNsSFJfS1hFZyIsImtpZCI6Im5PbzNaRHJPRFhFSzFqS1doWHNsSFJfS1hFZyJ9.eyJhdWQiOiIwMDAwMDAwMi0wMDAwLTAwMDAtYzAwMC0wMDAwMDAwMDAwMDAiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9lNTExNjUyNi01MWRjLTRjMTQtYjA4Ni1hNWNiNDcxNmJjNGIvIiwiaWF0IjoxNjE0MTEyNzU0LCJuYmYiOjE2MTQxMTI3NTQsImV4cCI6MTYxNDExNjY1NCwiYWlvIjoiRTJaZ1lMQmF1V25qcG12c2NhYlhJOTliSmt3c0FRQT0iLCJhcHBpZCI6IjIyNWVmMTU5LWFjZjAtNGRiNy04OGU0LWNlNDMyODYxOWM3MyIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0L2U1MTE2NTI2LTUxZGMtNGMxNC1iMDg2LWE1Y2I0NzE2YmM0Yi8iLCJyaCI6IjAuQVNBQUptVVI1ZHhSRkV5d2hxWExSeGE4UzFueFhpTHdyTGROaU9UT1F5aGhuSE1nQUFBLiIsInRlbmFudF9yZWdpb25fc2NvcGUiOiJFVSIsInRpZCI6ImU1MTE2NTI2LTUxZGMtNGMxNC1iMDg2LWE1Y2I0NzE2YmM0YiIsInV0aSI6Imo4bHRFZER5R2tDeURtdXR3QVVNQUEiLCJ2ZXIiOiIxLjAifQ.IeLADJiz5WRdQgf-3LfnUCfiQpKNjRIJjvDfYzoG9xgOQwBhSKeDlelIx0_FMx3oHtvYkGWebDy0Y1HjdrbgzoA2RTeIzS8IjylZcGfSuhA6kUvBa4JUPLW4Irefp3Bv77gUfS0dVzHVILADV-8VSCjivld7ovEANQagupsi4zhAyVWuNuHurDOSSI33lxnes-FmphUfiUfwmye9B676lwaj28I1dP3JxqFDDf3SNkjNLvTZyiDaIprZrt4TC_t5eopzqCL4X1ymnWxzJzMMPQGVOvhNEJj1oI_5VbRtoYdo_b5bYU5ZS7JSGcuOpog7vEtVk6uJDDT0MfQIuOLaeA"
  }
```

### Step 2 - Create a payment

To create a payment you need to send the specifications of that payment to Vipps, there is an extensive selection of options available which you can combine to make your custom payment experience. The required fields for a simple payment are:

| Parameter            | Type     | Required | Description                                                                   |
| -------------------- | -------- | -------- | ----------------------------------------------------------------------------- |
| `amount`             | `Object` | Y        | The `currency` and `value` of the payment in minor units                      |
| `paymentMethod`      | `Object` | Y        | The `type` of payment method you wish to process with                         |
| `reference`          | `string` | Y        | Your unique reference to this payment                                         |
| `returnUrl`          | `string` | Y        | The URL the user should be returned to after acting upon the payment          |
| `userFlow`           | `string` | Y        | The method to direct the user into the Vipps app to interact with the payment |
| `paymentDescription` | `string` | N        | The text shown to the user in the Vipps app with the payment                  |

To create a payment of 10 Norwegian Kroner send a request like the one below:

```bash
curl https://apitest.vipps.no/epayment/v1/payments\
-H "Authorization: Bearer <TOKEN>" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-H "Content-Type: application/json" \
-H "Idempotency-Key: UNIQUE-ID" \
-H "Merchant-Serial-Number: YOUR-MERCHANT-ACCOUNT-NUMBER" \
-X POST
-d '{
  "amount": {
    "currency": "NOK",
    "value": 1000
  },
  "paymentMethod": {
    "type": "WALLET"
  },
  "reference": "UNIQUE-PAYMENT-REFERENCE",
  "returnUrl": "https://yourwebsite.come/redirect?orderId=abcc123",
  "userFlow": "NATIVE_REDIRECT",
  "paymentDescription": "A simple payment"
}'
```

A valid request like the one above will result in a response with the following structure.

```json
{
  "aggregate": {
    "authorizedAmount": {
      "currency": "NOK",
      "type": "PURCHASE",
      "value": 0
    },
    "cancelledAmount": {
      "currency": "NOK",
      "type": "PURCHASE",
      "value": 0
    },
    "capturedAmount": {
      "currency": "NOK",
      "type": "PURCHASE",
      "value": 0
    },
    "refundedAmount": {
      "currency": "NOK",
      "type": "PURCHASE",
      "value": 0
    }
  },
  "amount": {
    "currency": "NOK",
    "type": "PURCHASE",
    "value": 1000
  },
  "authorisationType": "FINAL_AUTH",
  "state": "CREATED",
  "directCapture": false,
  "customerInteraction": "CUSTOMER_NOT_PRESENT",
  "paymentMethod": {
    "type": "WALLET"
  },
  "pspReference": "497f6eca-6276-4993-bfeb-53cbbbba6f08",
  "redirectUrl": "https://landing.vipps.no?token=abc123",
  "reference": "UNIQUE-PAYMENT-REFERENCE",
  "returnUrl": "https://yourwebsite.come/redirect?orderId=abcc123",
  "userFlow": "NATIVE_REDIRECT",
  "paymentDescription": "A simple payment"
}
```

The `redirectUrl` property should be used to direct the user to the Vipps app for completing the payment.

## Step 3 - Completing the payment

The user will be presented with the payment in the Vipps app where theyt can complete or reject the payment. Once the user has acted upon the payment they will be redirected back to the specified `returnUrl` under a "best effort" policy.

> Note: We cannot guarantee the user will be redirected back to the same browser or session, or that they will at all be redirected back. User interaction can be unpreditable and the user may choose to fully close the Vipps app or browser.

To receive the result of the users action you may either:

1. Poll the status of the payment view the
   [Get Payment][get-payment-endpoint] and
   [Get Payment Event Log[get-payment-event-log-endpoint] endpoints.
2. Receive status updates over our [Notification Webhooks](How-to-setup-Notification-Webhooks.md) service

### Polling

A request to the [Get Payment][get-payment-endpoint] URL will yield a response in the same structure as [Create Payment][create-payment-endpoint] specified above in [Step 2](#step-2---create-a-payment).

Example request:

```bash
curl https://apitest.vipps.no/epayment/v1/payments/UNIQUE-PAYMENT-REFERENCE \
-H "Authorization: Bearer <TOKEN>" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-X GET
```

To verify if a payment has been authorised by the user check if the `state` property is marked `AUTHORISED`. If the user has instead chosen to reject the payment or the user has instead chosen to click `cancel` on the landing page (card) the `state` property is marked `ABORTED`. If the user did not act within the payment expiration time then the `state` property is marked `EXPIRED`.

The payment event log can be fetched as such:

```bash
curl https://apitest.vipps.no/epayment/v1/payments/UNIQUE-PAYMENT-REFERENCE/events \
-H "Authorization: Bearer <TOKEN>" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-X GET
```

In the case the payment has been completed this will yield an array of events like such:

```json
[
  {
    "reference": "UNIQUE-PAYMENT-REFERENCE",
    "pspReference": "497f6eca-6276-4993-bfeb-53cbbbba6f08",
    "paymentAction": "CREATION",
    "success": true,
    "amount": {
      "currency": "NOK",
      "type": "PURCHASE",
      "value": 1000
    },
    "processedAt": "2021-02-24T14:15:22Z",
    "idempotencyKey": "IDEMPOTENCY-KEY-OF-REQUEST"
  },
  {
    "reference": "UNIQUE-PAYMENT-REFERENCE",
    "pspReference": "6037fc94-0205-4274-8b09-79cbb459658d",
    "paymentAction": "AUTHORISATION",
    "success": true,
    "amount": {
      "currency": "NOK",
      "type": "PURCHASE",
      "value": 1000
    },
    "authorisationType": "FINAL_AUTH",
    "processedAt": "2021-02-24T14:16:22Z",
    "idempotencyKey": "IDEMPOTENCY-KEY-OF-REQUEST"
  }
]
```

### Notification Events

If you are not dependent on getting the payment result immediately you may also use notification events to recieve the payment status update via our [Notification Webhooks](./How-to-setup-Notification-Webhooks.md) service. While we aim to deliver these event updates within a few seconds of the user completing the payment this service has an eventual delivery guarantee rather than imediate delivery.

> Note: this means we may deliver the same message several times to verify succesful delivery, use the `pspReference` field for duplicate delivery checking.

If you use the notification service you will recieve events in the same format as those in the array list returned from the [Get Payment Events](../TODO.md) endpoint.

For example a succeful authentication event would look like

```json
{
  "reference": "UNIQUE-PAYMENT-REFERENCE",
  "pspReference": "497f6eca-6276-4993-bfeb-53cbbbba6f08",
  "paymentAction": "CREATION",
  "success": true,
  "amount": {
    "currency": "NOK",
    "type": "PURCHASE",
    "value": 1000
  },
  "processedAt": "2021-02-24T14:15:22Z",
  "idempotencyKey": "IDEMPOTENCY-KEY-OF-REQUEST"
}
```

If the user had rejected or not acted upon the payment the event would look like

```json
{
  "reference": "UNIQUE-PAYMENT-REFERENCE",
  "pspReference": "38ab3a93-a819-4982-912d-089f3177e6c8",
  "paymentAction": "TERMINATION",
  "success": true,
  "amount": {
    "currency": "NOK",
    "type": "PURCHASE",
    "value": 1000
  },
  "processedAt": "2021-02-24T14:15:12Z",
  "idempotencyKey": "IDEMPOTENCY-KEY-OF-REQUEST"
}
```

## Step 4 - Capture the payment

Once the good or services are delivered or on their way to the customer it is time to capture the payment.
This can be done through the [Capture Payment][capture-payment-endpoint].
This endpoint take the following properties in the body of the request

| Parameter               | Type     | Required | Description                                                                                                                                 |
| ----------------------- | -------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `modificationAmount`    | `Object` | Y        | The `currency` and `value` of the modification in minor units. Must not be the entire amount, but cannot be more than the remaining amount. |
| `modificationReference` | `string` | N        | Your unique reference to this modification                                                                                                  |

An example capture would look like:

```bash
curl https://apitest.vipps.no/epayment/v1/payments/UNIQUE-PAYMENT-REFERENCE/capture \
-H "Authorization: Bearer <TOKEN>" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-H "Content-Type: application/json" \
-H "Idempotency-Key: UNIQUE-ID" \
-H "Merchant-Serial-Number: YOUR-MERCHANT-ACCOUNT-NUMBER" \
-X POST
-d '{
  "modificationAmount": {
    "currency": "NOK",
    "type": "PURCHASE",
    "value": 1000
  },
  "modificationReference": "UNIQUE-MODIFICATION-REFERENCE"
}'
```

Adjustments to a payment (capture, refund etc) as async.
You will get a `HTTP 202 Accepted` response with no body if the action is valid.
A callback will be sent once the capture is completed.
Additionally, polling on [Get Payment][get-payment-endpoint] can be done.
Once capture is completed the `Payment` object will be updated to reflect this.

In this case the `aggregate` property will be updated as such:

```json
{
  "aggregate": {
    "authorizedAmount": {
      "currency": "NOK",
      "type": "PURCHASE",
      "value": 1000
    },
    "cancelledAmount": {
      "currency": "NOK",
      "type": "PURCHASE",
      "value": 0
    },
    "capturedAmount": {
      "currency": "NOK",
      "type": "PURCHASE",
      "value": 1000
    },
    "refundedAmount": {
      "currency": "NOK",
      "type": "PURCHASE",
      "value": 0
    }
  }
}
```

## Next Steps

Now that you have completed your first payment,
we recommend you read further to better understand the full range of possibilities within the Vipps Merchant Payments API.

- [How to setup Notification Webhooks](./How-to-setup-Notification-Webhooks.md)
- [Payment modification, how to use cancel, capture and refund?](./Payment-Modification.md)
- [Using Vipps Merchant Payments in a shopper present context](./Customer-Present-Payments.md)

<!-- START_COMMENT -->
- [Profile sharing, requesting the users personal information](Profile-Sharing.md)
- [Logistics, how can I enable express checkout?](Logistics.md)
<!-- END_COMMENT -->

[create-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/CreatePayments/operation/createPayment
[get-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/QueryPayments/operation/getPayment
[get-payment-event-log-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/QueryPayments/operation/getPaymentEventLog
[cancel-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/cancelPayment
[capture-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/capturePayment
[refund-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/refundPayment
[adjust-authorization-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/adjustAuthorization
[force-approve-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/ForceApprove/operation/forceApprove
