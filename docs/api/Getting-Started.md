---
tags: [api]
---

# Getting Started with the Vipps Merchant Payments API


## Before you begin 

This document covers the quick steps for getting started with the Vipps Merchant Payments API. This document assumes you have signed up as a organisation with Vipps and have your test credentials from the [Merchant Portal](merchant-portal-how-to).

Once your merchant account is setup for Merchant Payments you should look at our [Configure Merchant Account]() page for available configuration options such as our [Notifications Webhooks]().

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
  "token_type": "Bearer"
  },
  "expires_in": "3599"
  },
  "ext_expires_in": "3599"
  },
  "expires_on": "1614116654"
  },
  "not_before": "1614112754"
  },
  "resource": "00000002-0000-0000-c000-000000000000"
  },
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Im5PbzNaRHJPRFhFSzFqS1doWHNsSFJfS1hFZyIsImtpZCI6Im5PbzNaRHJPRFhFSzFqS1doWHNsSFJfS1hFZyJ9.eyJhdWQiOiIwMDAwMDAwMi0wMDAwLTAwMDAtYzAwMC0wMDAwMDAwMDAwMDAiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9lNTExNjUyNi01MWRjLTRjMTQtYjA4Ni1hNWNiNDcxNmJjNGIvIiwiaWF0IjoxNjE0MTEyNzU0LCJuYmYiOjE2MTQxMTI3NTQsImV4cCI6MTYxNDExNjY1NCwiYWlvIjoiRTJaZ1lMQmF1V25qcG12c2NhYlhJOTliSmt3c0FRQT0iLCJhcHBpZCI6IjIyNWVmMTU5LWFjZjAtNGRiNy04OGU0LWNlNDMyODYxOWM3MyIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0L2U1MTE2NTI2LTUxZGMtNGMxNC1iMDg2LWE1Y2I0NzE2YmM0Yi8iLCJyaCI6IjAuQVNBQUptVVI1ZHhSRkV5d2hxWExSeGE4UzFueFhpTHdyTGROaU9UT1F5aGhuSE1nQUFBLiIsInRlbmFudF9yZWdpb25fc2NvcGUiOiJFVSIsInRpZCI6ImU1MTE2NTI2LTUxZGMtNGMxNC1iMDg2LWE1Y2I0NzE2YmM0YiIsInV0aSI6Imo4bHRFZER5R2tDeURtdXR3QVVNQUEiLCJ2ZXIiOiIxLjAifQ.IeLADJiz5WRdQgf-3LfnUCfiQpKNjRIJjvDfYzoG9xgOQwBhSKeDlelIx0_FMx3oHtvYkGWebDy0Y1HjdrbgzoA2RTeIzS8IjylZcGfSuhA6kUvBa4JUPLW4Irefp3Bv77gUfS0dVzHVILADV-8VSCjivld7ovEANQagupsi4zhAyVWuNuHurDOSSI33lxnes-FmphUfiUfwmye9B676lwaj28I1dP3JxqFDDf3SNkjNLvTZyiDaIprZrt4TC_t5eopzqCL4X1ymnWxzJzMMPQGVOvhNEJj1oI_5VbRtoYdo_b5bYU5ZS7JSGcuOpog7vEtVk6uJDDT0MfQIuOLaeA"
  }
}
```

### Step 2 - Create a payment

To create a payment you need to send the specifications of that payment to Vipps, there is an extensive selection of options available which you can combine to make your custom payment experience. The required fields for a simple payment are:

Parameter | Type | Required | Description
----------|------|----------|------------
`amount` | `Object` | Y | The `currency` and `value` of the payment in minor units
`merchantAccount` | `string` | Y | Your merchant account identifier
`paymentMethod` | `Object` | Y | The `type` of payment method you wish to process with
`reference` | `string` | Y | Your unique reference to this payment
`returnUrl` | `string` | Y | The URL the user should be returned to after acting upon the payment
`userFlow` | `string` | Y | The method to direct the user into the Vipps app to interact with the payment
`userText` | `string` | N | The text shown to the user in the Vipps app with the payment

To create a payment of 10 Norwegian Kroner send a request like the one below:

```bash
curl https://apitest.vipps.no/payments/v1 \
-H "Authorization: Bearer <TOKEN>" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-H "Content-Type: application/json" \
-H "Idempotency-Key: UNIQUE-ID" \
-X POST
-d '{
  "amount": {
    "currency": "NOK",
    "value": 1000
  },
  "merchantAccount": "YOUR-MERCHANT-ACCOUNT-NUMBER",
  "paymentMethod": {
    "type": "WALLET"
  },
  "reference": "UNIQUE-PAYMENT-REFERENCE",
  "returnUrl": "https://yourwebsite.come/redirect?orderId=abcc123",
  "userFlow": "NATIVE_REDIRECT",
  "userText": "A simple payment"
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
  "authorised": false,
  "autoCapture": false,
  "customerInteraction": "CUSTOMER_NOT_PRESENT",
  "merchantAccount": "YOUR-MERCHANT-ACCOUNT-NUMBER",
  "paymentMethod": {
    "type": "WALLET"
  },
  "pspReference": "497f6eca-6276-4993-bfeb-53cbbbba6f08",
  "redirectUrl": "https://landing.vipps.no?token=abc123",
  "reference": "UNIQUE-PAYMENT-REFERENCE",
  "returnUrl": "https://yourwebsite.come/redirect?orderId=abcc123",
  "userFlow": "NATIVE_REDIRECT",
  "userText": "A simple payment"
}
```

The `redirectUrl` property should be used to direct the user to the Vipps app for completing the payment.


## Step 3 - Completing the payment

The user will be presented with the payment in the Vipps app where theyt can complete or reject the payment. Once the user has acted upon the payment they will be redirected back to the specified `returnUrl` under a "best effort" policy. 

> Note: We cannot guarantee the user will be redirected back to the same browser or session, or that they will at all be redirected back. User interaction can be unpreditable and the user may choose to fully close the Vipps app or browser.

To receive the result of the users action you may either:

1. Poll the status of the payment view the [Get Payment]() and [Get Payment Event Log]() endpoints.
2. Receive status updates over our [Notification Webhooks]() service

### Polling

A request to the [Get Payment]() URL will yield a response in the same structure as [Create Payment]() specified above in [Step 2](#Step-2---Create-a-payment). 

Example request:
```bash
curl https://apitest.vipps.no/payments/v1/UNIQUE-PAYMENT-REFERENCE \
-H "Authorization: Bearer <TOKEN>" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-X GET
```

To verify if a payment has been authorised by the user check the `authorised` property. If the user has instead chosed to reject the payment the `aggregate.cancelledAmount.value` will be equal to the `amount.value` originally specified in the Create Payment request.


The payment event log can be fetched as such:
```bash
curl https://apitest.vipps.no/payments/v1/UNIQUE-PAYMENT-REFERENCE/events \
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
    "paymentAction": "CREATED",
    "amount": {
      "currency": "NOK",
      "type": "PURCHASE",
      "value": 1000
    },
    "processedAt": "2021-02-24T14:15:22Z"
  },
  {
    "reference": "UNIQUE-PAYMENT-REFERENCE",
    "pspReference": "6037fc94-0205-4274-8b09-79cbb459658d",
    "paymentAction": "AUTHORISATION",
    "amount": {
      "currency": "NOK",
      "type": "PURCHASE",
      "value": 1000
    },
    "authorisationType": "FINAL_AUTH",
    "processedAt": "2021-02-24T14:16:22Z"
  }
]
```

### Notification Events

If you are not dependent on getting the payment result immediately you may also use notification events to recieve the payment status update via our [Notification Webhooks]() service. While we aim to deliver these event updates within a few seconds of the user completing the payment this service has an eventual delivery guarantee rather than imediate delivery. 

> Note: this means we may deliver the same message several times to verify succesful delivery, use the `pspReference` field for duplicate delivery checking.

If you use the notification service you will recieve events in the same format as those in the array list returned from the [Get Payment Events]() endpoint.


For example a succeful authentication event would look like

```json
{
  "reference": "UNIQUE-PAYMENT-REFERENCE",
  "pspReference": "497f6eca-6276-4993-bfeb-53cbbbba6f08",
  "paymentAction": "CREATED",
  "amount": {
    "currency": "NOK",
    "type": "PURCHASE",
    "value": 1000
  },
  "processedAt": "2021-02-24T14:15:22Z"
}
```

If the user had rejected or not acted upon the payment the event would look like

```json
{
  "reference": "UNIQUE-PAYMENT-REFERENCE",
  "pspReference": "38ab3a93-a819-4982-912d-089f3177e6c8",
  "paymentAction": "TERMINATE",
  "amount": {
    "currency": "NOK",
    "type": "PURCHASE",
    "value": 1000
  },
  "processedAt": "2021-02-24T14:15:12Z"
}
```

## Step 4 - Capture the payment

Once the good or services are delivered or on their way to the customer it is time to capture the payment.
This can be done through the [Capture Payment]().
This endpoint take the following properties in the body of the request

Parameter | Type | Required | Description
----------|------|----------|------------
`merchantAccount` | `string` | Y | Your merchant account identifier
`modificationAmount` | `Object` | Y | The `currency` and `value` of the modification in minor units. Must not be the entire amount, but cannot be more than the remaining amount.
`modificationReference` | `string` | N | Your unique reference to this modification

An example capture would look like:

```bash
curl https://apitest.vipps.no/payments/v1/UNIQUE-PAYMENT-REFERENCE/capture \
-H "Authorization: Bearer <TOKEN>" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-H "Content-Type: application/json" \
-H "Idempotency-Key: UNIQUE-ID" \
-X POST
-d '{
  "merchantAccount": "YOUR-MERCHANT-ACCOUNT-NUMBER",
  "modificationAmount": {
    "currency": "NOK",
    "type": "PURCHASE",
    "value": 1000
  },
  "modificationReference": "UNIQUE-MODIFICATION-REFERENCE"
}'
```

In reponse you will get the payment object you get from the [Create Payment]() and [Get Payment]() endpoints, updated to reflect the modification performed.

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

# Next Steps

Now that you have completed your first payment we recommend you read further to better understand the full range of possibilities within the Vipps Merchant Payments API.

* [How to setup Notification Webhooks]()
* [Payment modification, how to use cancel, capture and refund?]()
* [Profile sharing, requesting the users personal information]()
* [Logistics, how can I enable express checkout?]()
* [Using Vipps Merchant Payments in a shopper present context]()


