---
title: Capture the payment with the ePayment API
sidebar_label: Capture
id: capture
sidebar_position: 20
---

# Capture a payment

See
[Common API topics - Reserve and Capture](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/reserve-and-capture)
for a general introduction to reservations and captures.

A capture can be made in full, or partially if desired. The capture amount must
be defined in capture API request.

Captured funds will be settled to the merchants settlement account after two
business days. See
[Settlement Information](https://developer.vippsmobilepay.com/docs/vipps-developers/settlements)
for more details.

## Capture via the API

Once the goods or services are delivered or on their way to the customer it is time to capture the payment.
This can be done through the
[`POST:/payments/{reference}/capture`][capture-payment-endpoint].

An example of a capture request body:

```json
{
   "modificationAmount":{
      "currency":"NOK",
      "value":1000
   }
}
```

In the response,  the `aggregate` object will be updated to reflect the capture, for example:


```json
{
   "aggregate":{
      "authorizedAmount":{
         "currency":"NOK",
         "value":1000
      },
      "cancelledAmount":{
         "currency":"NOK",
         "value":0
      },
      "capturedAmount":{
         "currency":"NOK",
         "value":1000
      },
      "refundedAmount":{
         "currency":"NOK",
         "value":0
      }
   }
}
```

A notification will also be sent once the capture is completed if a [webhook](../features/webhooks.md) is registered for the event `epayments.payment.captured.v1`.

## Partial Capture

If you do not wish to capture the entire amount a smaller amount than authorized can be captured. This can be done multiple times.

The `Idempotency-Key` header is there to help you ensure at most once operation where needed.

An example of a partial capture request body:

```json
{
   "modificationAmount":{
      "currency":"NOK",
      "value":250
   }
}
```

Once capture is completed the `aggregate` will be updated to reflect this, for example:

```json
{
   "aggregate":{
      "authorizedAmount":{
         "currency":"NOK",
         "value":1000
      },
      "cancelledAmount":{
         "currency":"NOK",
         "value":0
      },
      "capturedAmount":{
         "currency":"NOK",
         "value":250
      },
      "refundedAmount":{
         "currency":"NOK",
         "value":0
      }
   }
}
```

If you are not going to capture the rest of the authorized amount you should
[cancel](cancel.md#cancel-after-a-partial-capture) the remaining amount.

[capture-payment-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/AdjustPayments/operation/capturePayment