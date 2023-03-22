<!-- START_METADATA
---
title: Free Standing Card Payments
hide_table_of_contents: true
pagination_next: null
pagination_prev: APIs/epayment-api/quick-start
sidebar_position: 40
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

END_METADATA -->

# Free Standing Card Payments

ePayments API supports Free Standing Card Payments, where any user can pay regardless of having the Vipps app installed.


## Create a Free Standing Card Payment
Remember to have a fresh access token, see 
[Setup and Authorize](../quick-start.md#step-1---setup).
Then, call the [Create Payment][create-payment-endpoint] endpoint with `paymentMethod.type = "CARD"`.


<Tabs
defaultValue="postman"
groupId="sdk-choice"
values={[
{label: 'Postman', value: 'postman'},
{label: 'curl', value: 'curl'},
]}>
<TabItem value="postman">

```bash
Send request Create Payment - Card Payment
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
    "type": "CARD"
  },
  "reference": "UNIQUE-PAYMENT-REFERENCE",
  "returnUrl": "https://yourwebsite.come/redirect?reference=abcc123",
  "userFlow": "WEB_REDIRECT",
  "paymentDescription": "A simple free standing card payment"
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
        PaymentMethod = new PaymentMethod { Type = PaymentMethodType.CARD },
        UserFlow = CreatePaymentRequestUserFlow.WEB_REDIRECT,
        Reference = reference,
        PaymentDescription = "A simple free standing card payment",
        ReturnUrl = $"https://no.where.com/{reference}"
    };
var createPaymentResult = await EpaymentService.CreatePayment(createPaymentRequest);
```

</TabItem>
</Tabs>


## Complete the payment

The result of this request will contain a `redirectUrl` pointing to the card entry page.

:::note
The card entry page currently is not currently available in the test environment.
:::

![Enter card details](../images/vipps-ecom-pay-by-card-step2.png)

On successful payment, the user is redirected back to the merchant’s store, and the order is confirmed.



[create-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/CreatePayments/operation/createPayment
