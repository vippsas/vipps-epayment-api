<!-- START_METADATA
---
title: Refunding a Payment
id: refund
sidebar_position: 30
pagination_prev: APIs/epayment-api/api-guide/modifications/capture
pagination_next: Null
---

import ApiSchema from '@theme/ApiSchema';

END_METADATA -->

# Refunding a payment

A [Refund][refund-payment-endpoint] will reverse the direction of a transaction and move money from the Merchant back to the customer.

Refunds can be made in full or partially as needed. The refund amount must be defined in the refund API request.

Refunded funds will be deducted from the merchants settlement account after two business days. See [Settlement Information](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/settlements) for more details.

## Refund via the API

If a customer has returned the goods or the service is not delivered you should refund the payment.
This can be done through the [Refund Payment Endpoint][refund-payment-endpoint].
This endpoint take the following properties in the body of the request

<ApiSchema id="epayment-swagger-id" pointer="#/components/schemas/RefundModificationRequest" />

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

Response:

<ApiSchema id="epayment-swagger-id" pointer="#/components/schemas/ModificationResponse" />

A notification will also be sent once the modification is completed if a webhook is registered.

After refund the `aggregate` object will be updated to reflect this, for example:

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

Once capture is completed the `aggregate` object will be updated to reflect this, for example:

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
