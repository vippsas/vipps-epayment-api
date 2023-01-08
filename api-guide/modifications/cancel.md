---
title: Cancelling a Payment
id: cancel
pagination_prev: APIs/epayment-api/api-guide/getting-started
pagination_next: null
---

import ApiSchema from '@theme/ApiSchema';

If you no longer wish to initiate settlement of the remaining funds on a payment then you should [Cancel](cancel-payment-endpoint) the payment. Cancelling a payment provides a good user experience and synchronizes the users bank statement and Vipps payment overview with their expectations from a merchant.

A payment can be cancelled via the api or merchant portal at any point until the payment is fully captured. A cancellation will release any remaining authorised funds on the customers bank account. If the payment has not yet reached the `AUTHORIZED` state a cancellation by the merchant via the api will result in `TERMINATED` state of the payment.

:::info
It is not possible to cancel a payment while a user is actively authorizing the payment. Eg: The payment is under processing with the payment scheme, or the user is in a 3DSecure session.
:::


## Cancelling via the API

The payment can be cancelled through the [Cancel Payment Enpoint][cancel-payment-endpoint].

An example cancel would look like:

```bash
curl https://apitest.vipps.no/epayment/v1/payments/UNIQUE-PAYMENT-REFERENCE/cancel \
-H "Authorization: Bearer <TOKEN>" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-H "Content-Type: application/json" \
-H "Idempotency-Key: UNIQUE-ID" \
-H "Merchant-Serial-Number: YOUR-MERCHANT-ACCOUNT-NUMBER" \
-X POST
```

Response:

<ApiSchema id="epayment-swagger-id" pointer="#/components/schemas/ModificationResponse" />

A notification will also be sent once the modification is completed if a webhook is registered.

After cancellation the `aggregate` object will be updated to reflect this, for example:

```json
{
  "aggregate": {
    "authorizedAmount": {
      "currency": "NOK",
      "value": 1000
    },
    "cancelledAmount": {
      "currency": "NOK",
      "value": 1000
    },
    "capturedAmount": {
      "currency": "NOK",
      "value": 0
    },
    "refundedAmount": {
      "currency": "NOK",
      "value": 0
    }
  }
}
```

## Cancel after a partial capture

If you have captured a partial amount of the authorised amount and will not capture the remaining amount you should release this. The cancel endpoint will do this for you when called.

For exmaple we wish to release the remaining funds of a payment with the following aggregate state:

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
      "value": 250
    },
    "refundedAmount": {
      "currency": "NOK",
      "value": 0
    }
  }
}
```

We would then call [Cancel](cancel-payment-endpoint) as normal

```bash
curl https://apitest.vipps.no/epayment/v1/payments/UNIQUE-PAYMENT-REFERENCE/cancel \
-H "Authorization: Bearer <TOKEN>" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-H "Content-Type: application/json" \
-H "Idempotency-Key: UNIQUE-ID" \
-H "Merchant-Serial-Number: YOUR-MERCHANT-ACCOUNT-NUMBER" \
-X POST
```

The aggregate would then be updated to reflect the modification and the funds release in the users bank account

```json
{
  "aggregate": {
    "authorizedAmount": {
      "currency": "NOK",
      "value": 1000
    },
    "cancelledAmount": {
      "currency": "NOK",
      "value": 750
    },
    "capturedAmount": {
      "currency": "NOK",
      "value": 250
    },
    "refundedAmount": {
      "currency": "NOK",
      "value": 0
    }
  }
}
```

:::note
Cancelling a payment will always cancel the entire remaining not-captured amount, this is not reversable.
:::


[cancel-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/cancelPayment
