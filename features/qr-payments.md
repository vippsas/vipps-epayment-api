<!-- START_METADATA
---
title: QR Payments
hide_table_of_contents: true
pagination_next: null
pagination_prev: APIs/epayment-api/quick-start
sidebar_position: 50
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

END_METADATA -->

# QR Payments

ePayments API supports QR payments directly, making it even easier to provide
[One-Time payment QR](https://vippsas.github.io/vipps-developer-docs/docs/vipps-solutions/qr-code-print).


## Create a QR Payment
Remember to have a fresh access token, see
[Setup and Authorize](../quick-start.md#step-1---setup).
Then, call the [Create Payment][create-payment-endpoint] endpoint with `userFlow = "QR"`

<Tabs
defaultValue="postman"
groupId="sdk-choice"
values={[
{label: 'Postman', value: 'postman'},
{label: 'curl', value: 'curl'},
]}>
<TabItem value="postman">

```bash
Send request Create Payment - QR
```

</TabItem>
<TabItem value="curl">

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
  "userFlow": "QR",
  "paymentDescription": "A simple QR payment",
  "qrFormat": {
    "format": "IMAGE/SVG+XML",
    "size": 1024
  }
}'
```

</TabItem>
<TabItem value="csharp">

```csharp
var reference = Guid.NewGuid().ToString();
var createPaymentRequest = new CreatePaymentRequest
    {
        Amount = new Amount
        {
            Currency = Currency.NOK,
            Value = 1000 // 1000 øre = 10 KR
        },
        PaymentMethod = new PaymentMethod { Type = PaymentMethodType.WALLET },
        UserFlow = CreatePaymentRequestUserFlow.QR,
        Reference = reference,
        PaymentDescription = "A QR payment",
        ReturnUrl = $"https://no.where.com/{reference}"
    };
var createPaymentResult = await EpaymentService.CreatePayment(createPaymentRequest);
```

</TabItem>
</Tabs>


## Complete the payment

The result of this request will contain a `redirectUrl` pointing to a link where you can download the QR image.
Simply scan the image with your mobile device, and the Vipps app will automatically open, where you can approve the payment.

[create-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/CreatePayments/operation/createPayment
