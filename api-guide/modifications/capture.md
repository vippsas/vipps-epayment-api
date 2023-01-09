<!-- START_METADATA
---
title: Capturing a Payment
id: capture
pagination_prev: APIs/epayment-api/api-guide/getting-started
pagination_next: APIs/epayment-api/api-guide/modifications/refund
---
import ApiSchema from '@theme/ApiSchema';

END_METADATA -->

# Capture

When a payment is initiated with `$.directCapture = false` you must [Capture][capture-payment-endpoint] a payment in order to initiate settlement of the authorised funds.

Captured funds will be settled to the merchants settlement account after two business days. See [Settlement Information](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/settlements) for more details.

A capture can be made in full, or partially if desired. The capture amount must be defined in capture API request.

## Capture via the API

Once the good or services are delivered or on their way to the customer it is time to capture the payment.
This can be done through the [Capture Payment Endpoint][capture-payment-endpoint].
This endpoint take the following properties in the body of the request

<ApiSchema id="epayment-swagger-id" pointer="#/components/schemas/CaptureModificationRequest" />

An example capture would look like:

```bash
curl https://apitest.vipps.no/epayment/v1/payments/UNIQUE-PAYMENT-REFERENCE/capture \
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

After capture the `aggregate` object will be updated to reflect this, for example:

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
      "value": 0
    }
  }
}
```

## Partial Capture

If you do not wish to capture the entire amount a smaller amount than authorised can be captured. This can be done multiple times.

The `Idempotency-Key` header is there to help you ensure at most once operation where needed.

An example partial capture would look like:

```bash
curl https://apitest.vipps.no/epayment/v1/payments/UNIQUE-PAYMENT-REFERENCE/capture \
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
      "value": 250
    },
    "refundedAmount": {
      "currency": "NOK",
      "value": 0
    }
  }
}
```

If you are not going to capture the rest of the authorized amount you should [cancel](cancel.md#cancel-after-a-partial-capture) the remaining amount.

[capture-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/capturePayment
