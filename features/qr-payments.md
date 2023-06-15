---
title: QR Payments
hide_table_of_contents: true
sidebar_position: 50
---


# QR Payments

The ePayment API supports QR payments directly, making it easy to provide
[One-Time payment QR](https://developer.vippsmobilepay.com/docs/vipps-solutions/qr-code-print).

## Create a QR Payment

Remember to have a fresh access token, see
[Set up and Authorize](../quick-start.md#step-1---setup).
Then, call the [Create Payment][create-payment-endpoint] endpoint with `userFlow = "QR"`.

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
    "value": 1000
  },
  "paymentMethod": {
    "type": "WALLET"
  },
  "reference": "acme-shop-123-order123abc,
  "returnUrl": "https://example.com/redirect?reference=acme-shop-123-order123abc",
  "userFlow": "QR",
  "paymentDescription": "Two pairs of socks, paid with a QR code",
  "qrFormat": {
    "format": "IMAGE/SVG+XML",
    "size": 1024
  }
}'
```

## Complete the payment

The result of this request will contain a `redirectUrl` pointing to a link where you can download the QR image.
Simply scan the image with your mobile device and the Vipps app will automatically open, where you can approve the payment.

[create-payment-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments/operation/createPayment
