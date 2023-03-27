<!-- START_METADATA
---
title: Cancel the payment with the ePayment API
sidebar_label: Cancel
id: cancel
sidebar_position: 20
description: Cancel payment with the ePayment API.
---

END_METADATA -->

# Cancel a payment

See
[Common API topics - Cancellations](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/common-topics/cancel)
for a general introduction to cancellations.

If you no longer wish to initiate settlement of the remaining funds on a payment then you should [Cancel][cancel-payment-endpoint] the payment. Cancelling a payment provides a good user experience and synchronizes the users bank statement and Vipps payment overview with their expectations from a merchant.

A payment can be cancelled via the api or merchant portal at any point until the payment is fully captured. A cancellation will release any remaining authorized funds on the customers bank account.

## Cancel via the API

The payment can be cancelled through the [Cancel Payment Endpoint][cancel-payment-endpoint].

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


## Cancel before Authorization
If the payment has not yet reached the `AUTHORIZED` state a cancellation by the merchant via the api will result in `TERMINATED` state of the payment.
An example response for a cancel request pre-authorization looks like this
```json
{
    "amount": {
        "currency": "NOK",
        "value": 1000
    },
    "state": "TERMINATED",
    "aggregate": {
        "authorizedAmount": {
            "currency": "NOK",
            "value": 0
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
    },
    "pspReference": "70b6ebe0-b400-4652-a3ea-70645e025035",
    "reference": "UNIQUE-PAYMENT-REFERENCE"
}
```

After the cancel is complete, a notification will be sent if a [webhook](../features/webhooks.md) is registered for the event `epayments.payment.terminated.v1`.

## Cancel after Authorization
If the payment has reached the `AUTHORIZED` state a cancellation by the merchant via the api will **not** change the state of the payment, it will remain `AUTHORIZED`.
However, the reserved amount from the customer will be released, and the `aggregate` object will reflect this with the `cancelledAmount` property.
An example response for a cancel request Post-authorization looks like this:
```json
{
    "amount": {
        "currency": "NOK",
        "value": 1000
    },
    "state": "AUTHORIZED",
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
    },
    "pspReference": "37c34d8c-2649-448e-864b-060d5d93e4c4",
    "reference": "UNIQUE-PAYMENT-REFERENCE"
}
```

Even though the state of a payment doesn't change, a notification will be sent after the cancellation is processed, if a [webhook](../features/webhooks.md) is registered for the event `epayments.payment.cancelled.v1`.

:::info
It is not possible to cancel a payment while a user is actively authorizing the payment. Eg: The payment is under processing with the payment scheme, or the user is in a 3DSecure session.
:::




## Cancel after a partial capture

If you have captured a partial amount of the authorized amount and will not capture the remaining amount you should release this. The cancel endpoint will do this for you when called.

For example we wish to release the remaining funds of a payment with the following aggregate state:

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

We would then call [Cancel][cancel-payment-endpoint] as normal

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
Cancelling a payment will always cancel the entire remaining not-captured amount, this is not reversible.
:::

[cancel-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/cancelPayment
