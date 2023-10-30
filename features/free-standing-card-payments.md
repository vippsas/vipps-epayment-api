---
hide_table_of_contents: true
sidebar_position: 40
---

# Freestanding card payments

The ePayment API supports freestanding card payments, where any user can pay with a card without
using the app. This lets merchants accept payments from customers in countries where
the Vipps or MobilePay app is not yet available.

See
[FAQ: Card payments](https://developer.vippsmobilepay.com/docs/knowledge-base/payments#card-payments).

## Create a freestanding card payment

Remember to have a fresh access token, see
[Setup and Authorize](../quick-start.md#step-1---setup).
Then, call the [Create Payment][create-payment-endpoint] endpoint with `paymentMethod.type = "CARD"`.

To initiate a freestanding card payment, the
[`POST:/epayment/v1/payments`](https://developer.vippsmobilepay.com/api/epayment/#tag/CreatePayments/operation/createPayment)
request must specify `paymentMethod.type = "CARD"`.

```bash
curl https://apitest.vipps.no/epayment/v1/payments \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni (truncated)" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Content-Type: application/json" \
-H "Idempotency-Key: 49ca711a-acee-4d01-993b-9487112e1def" \
-H "Merchant-Serial-Number: 123456" \
-X POST \
-d '{
  "amount": {
    "currency": "NOK",
    "value": 49900
  },
  "paymentMethod": {
    "type": "CARD"
  },
  "reference": "acme-shop-123-order123abc",
  "returnUrl": "https://example.com/redirect?reference=acme-shop-123-order123abc",
  "userFlow": "WEB_REDIRECT",
  "paymentDescription": "Two pairs of socks"
}'
```

## Complete the payment

The response for the request above will contain a `redirectUrl` pointing to the web page with a form
where the user enters the payment card details.

:::note
The card entry page currently is not currently available in the
[test environment](https://developer.vippsmobilepay.com/docs/test-environment/).
:::

![Enter card details](../images/vipps-ecom-pay-by-card-step2.png)

After confirming the payment in the app, the user is redirected back to the merchant’s store,
where the order confirmation is shown.

[create-payment-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments/operation/createPayment
