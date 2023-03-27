<!-- START_METADATA
---
title: Refund the payment with the ePayment API
sidebar_label: Refund
id: refund
sidebar_position: 30
pagination_next: Null
---

END_METADATA -->

# Refund a payment

See
[Common API topics - Refunds](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/common-topics/refund)
for a general introduction to refunds.


A [Refund][refund-payment-endpoint] will reverse the direction of a transaction and move money from the Merchant back to the customer. 

Refunds can be made in full or partially as needed. The refund amount must be defined in the refund API request.

Refunded funds will be deducted from the merchants settlement account after two business days. See [Settlement Information](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/settlements) for more details.

## Refund via the API

If a customer has returned the goods or the service is not delivered you should refund the payment.
This can be done through the [Refund Payment Endpoint][refund-payment-endpoint].

An example refund would look like:

```bash
curl https://apitest.vipps.no/epayment/v1/payments/UNIQUE-PAYMENT-REFERENCE/refund \
-H "Authorization: Bearer <TOKEN>" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-H "Content-Type: application/json" \
-H "Idempotency-Key: UNIQUE-ID" \
-H "Merchant-Serial-Number: YOUR-MERCHANT-ACCOUNT-NUMBER" \
-X POST \
-d '{
  "modificationAmount": {
    "currency": "NOK",
    "value": 1000
  }
}'
```



In the response, the `aggregate` object will be updated to reflect the refunded amount, for example:

```json
{
  "aggregate": {
    "authorizedAmount": {
      "currency": "NOK",
      "value": 1000
    },
    "cancelledAmount": {
      "currency": "NOK",
      "value": 0
    },
    "capturedAmount": {
      "currency": "NOK",
      "value": 1000
    },
    "refundedAmount": {
      "currency": "NOK",
      "value": 1000
    }
  }
}
```

A notification will also be sent once the refund is completed if a [webhook](../features/webhooks.md) is registered for the event `epayments.payment.refunded.v1`.

## Partial Refund

If you do not wish to refund the entire amount a smaller amount than captured can be refunded. This can be done multiple times.

The `Idempotency-Key` header is there to help you ensure at most once operation where needed.

An example partial refund would look like:

```bash
curl https://apitest.vipps.no/epayment/v1/payments/UNIQUE-PAYMENT-REFERENCE/refund \
-H "Authorization: Bearer <TOKEN>" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-H "Content-Type: application/json" \
-H "Idempotency-Key: UNIQUE-ID" \
-H "Merchant-Serial-Number: YOUR-MERCHANT-ACCOUNT-NUMBER" \
-X POST \
-d '{
  "modificationAmount": {
    "currency": "NOK",
    "value": 250
  }
}'
```

Once refund is completed the `aggregate` object will be updated to reflect the partial refund, for example:

```json
{
  "aggregate": {
    "authorizedAmount": {
      "currency": "NOK",
      "value": 1000
    },
    "cancelledAmount": {
      "currency": "NOK",
      "value": 0
    },
    "capturedAmount": {
      "currency": "NOK",
      "value": 1000
    },
    "refundedAmount": {
      "currency": "NOK",
      "value": 250
    }
  }
}
```

[refund-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/refundPayment
