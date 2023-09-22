---
title: Refund the payment with the ePayment API
sidebar_label: Refund
id: refund
sidebar_position: 50
---


# Refund a payment

See
[Common API topics: Refunds](https://developer.vippsmobilepay.com/docs/common-topics/refund)
for a general introduction to refunds.

A [Refund][refund-payment-endpoint] will reverse the direction of a transaction and move money from the Merchant back to the customer.

Refunds can be made in full or partially as needed. The refund amount must be defined in the refund API request.

Refunded funds will be deducted from the merchant's settlement account after two business days. See [Settlement Information](https://developer.vippsmobilepay.com/docs/settlements) for more details.

## Refund via the API

If a customer has returned the goods or the service is not delivered you should refund the payment.
This can be done through the [Refund Payment Endpoint][refund-payment-endpoint].

An example refund would look like:

```bash
curl https://apitest.vipps.no/epayment/v1/payments/UNIQUE-PAYMENT-REFERENCE/refund \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni (truncated)" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Content-Type: application/json" \
-H "Idempotency-Key: 49ca711a-acee-4d01-993b-9487112e1def" \
-H "Merchant-Serial-Number: 123456" \
-X POST \
-d '{
  "modificationAmount": {
    "currency": "NOK",
    "value": 10000
  }
}'
```

In the response, the `aggregate` object will be updated to reflect the refunded amount, for example:

```json
{
  "aggregate": {
    "authorizedAmount": {
      "currency": "NOK",
      "value": 49900
    },
    "cancelledAmount": {
      "currency": "NOK",
      "value": 0
    },
    "capturedAmount": {
      "currency": "NOK",
      "value": 10000
    },
    "refundedAmount": {
      "currency": "NOK",
      "value": 10000
    }
  }
}
```

A notification will also be sent once the refund is completed if a
[webhook](../features/webhooks.md)
is registered for the event `epayments.payment.refunded.v1`.

## Partial Refund

If you do not wish to refund the entire amount a smaller amount than captured can be refunded. This can be done multiple times.

The `Idempotency-Key` header is there to help you ensure at most once operation where needed.

An example partial refund would look like:

```bash
curl https://apitest.vipps.no/epayment/v1/payments/UNIQUE-PAYMENT-REFERENCE/refund \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni (truncated)" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Content-Type: application/json" \
-H "Idempotency-Key: 49ca711a-acee-4d01-993b-9487112e1def" \
-H "Merchant-Serial-Number: 123456" \
-X POST \
-d '{
  "modificationAmount": {
    "currency": "NOK",
    "value": 10000
  }
}'
```

Once refund is completed the `aggregate` object will be updated to reflect this, for example:

```json
{
  "aggregate": {
    "authorizedAmount": {
      "currency": "NOK",
      "value": 49900
    },
    "cancelledAmount": {
      "currency": "NOK",
      "value": 0
    },
    "capturedAmount": {
      "currency": "NOK",
      "value": 10000
    },
    "refundedAmount": {
      "currency": "NOK",
      "value": 10000
    }
  }
}
```

[refund-payment-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/AdjustPayments/operation/refundPayment
