---
hide_table_of_contents: true
sidebar_position: 40
---

# Freestanding card payments

The ePayment API supports freestanding card payments, where any user can pay regardless of having the Vipps app installed.

## Create a freestanding card payment

Remember to have a fresh access token (see
[Setup and Authorize](../quick-start.md#step-1---setup)).
Then, call the [Create Payment][create-payment-endpoint] endpoint with `paymentMethod.type = "CARD"`.

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
  "paymentDescription": "A simple freestanding card payment"
}'
```

## Complete the payment

The result of this request will contain a `redirectUrl` pointing to the card entry page.

:::note
The card entry page currently is not currently available in the test environment.
:::

![Enter card details](../images/vipps-ecom-pay-by-card-step2.png)

On successful payment, the user is redirected back to the merchant’s store, and the order is confirmed.



[create-payment-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments/operation/createPayment
