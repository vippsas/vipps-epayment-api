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
[Setup and Authorize](../quick-start.md#step-1---setup).
Then, call the [Create Payment][create-payment-endpoint] endpoint with `userFlow = "QR"`.

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


## Complete the payment

The result of this request will contain a `redirectUrl` pointing to a link where you can download the QR image.
Simply scan the image with your mobile device, and the Vipps app will automatically open, where you can approve the payment.

[create-payment-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments/operation/createPayment
