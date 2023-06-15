---
title: Customer present payments
hide_table_of_contents: true
sidebar_position: 30
---

# Customer present payments

When using the ePayment API to process payments in a physical setting (e.g., from a Point of Sale
device or in a restaurant), the
[`POST:/epayment/v1/payments`](https://developer.vippsmobilepay.com/api/epayment/#tag/CreatePayments/operation/createPayment)
request must specify `"customerInteraction": "CUSTOMER_PRESENT"`.

The `CUSTOMER_PRESENT` parameter is required for compliance and reporting reasons, as
the risk is lower when the customer is physically present than when the customer is
paying remotely over the net.

Example request:

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
  "customerInteraction": "CUSTOMER_PRESENT",
  "reference": "acme-shop-123-order123abc",
  "returnUrl": "https://example.com/redirect?reference=acme-shop-123-order123abc",
  "userFlow": "WEB_REDIRECT",
  "paymentDescription": "Two pairs of socks"
}'
```


