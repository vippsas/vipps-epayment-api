---
title: Create the payment with the ePayment API
sidebar_label: Create
id: create
sidebar_position: 10
description: Create payment with the ePayment API.
---


# Create payment

The first step in the payment flow is to create a payment with
[`POST:/epayment/v1/payments`](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments).
This endpoint supports both *wallet* (i.e., the Vipps or MobilePay app) and
freestanding card payments, when card details are entered.
Each payment type offers separate user flows.

```mermaid
flowchart TD
    Payment --> Wallet[fa:fa-mobile paymentMethod: WALLET]
    Wallet --> WEB_REDIRECT[fa:fa-desktop userFlow: WEB_REDIRECT]
    Wallet --> NATIVE_REDIRECT[fa:fa-mobile userFlow: NATIVE_REDIRECT]
    Wallet --> PUSH_MESSAGE[fa:fa-bell userFlow: PUSH_MESSAGE]
    Wallet --> QR[fa:fa-qrcode userFlow: QR]
    Payment --> Card[fa:fa-credit-card paymentMethod:CARD]
    Card --> CARD_WEB_REDIRECT[fa:fa-desktop userFlow: WEB_REDIRECT]
```

The `paymentMethod.type` in the request determines the type of payment:

* `WALLET`: The user pays through the Vipps or MobilePay app. This includes delegated Secure Customer Authentication (SCA),
   where the login to the app eliminates the need for a separate SCA step.
  `WALLET` payments include retry functionality such that, if the user attempts to pay
  with a declined card, they can retry with a different card for the same payment process.
* `CARD`: The user pays with a card. They enter the card details into a form and then complete the 3D Secure step-up
  for SCA. See
  [Card payments](https://developer.vippsmobilepay.com/docs/knowledge-base/payments#card-payments)
  for more information.

  **Please note:** Card payment (`CARD`) is not available in the test environment.


<details>
<summary>General create request example</summary>
<div>

Here is an example HTTP POST:

[`POST:/epayment/v1/payments`](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments/operation/createPayment)

```bash
curl https://apitest.vipps.no/epayment/v1/payments \
-H "Content-Type: application/json" \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <truncated>" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Merchant-Serial-Number: 123456" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-H "Vipps-System-Plugin-Name: acme-webshop" \
-H "Vipps-System-Plugin-Version: 4.5.6" \
-H "Idempotency-Key: 49ca711a-acee-4d01-9773b-9487112e1def" \
-X POST \
-d '{
      "amount":{
         "currency":"NOK",
         "value":6000
      },
      "customer":{
         "phoneNumber":4791234567
      },
      "paymentMethod":{
         "type":"WALLET"
      },
      "receipt":{
         "orderLines": [
            {
               "name": "Socks",
               "id": "line_item_1",
               "totalAmount": 1000,
               "totalAmountExcludingTax": 800,
               "totalTaxAmount": 200,
               "taxPercentage": 25,
               "unitInfo": {
               "unitPrice": 400,
               "quantity": "2.5",
               "quantityUnit": "KG"
               },
               "discount": 0,
               "productUrl": "https://example.com/store/socks",
               "isReturn": false,
               "isShipping": false
            },
            {
               "name": "Flip-flops",
               "id": "line_item_2",
               "totalAmount": 5000,
               "totalAmountExcludingTax": 4000,
               "totalTaxAmount": 1000,
               "taxPercentage": 25,
               "unitInfo": {
               "unitPrice": 2500,
               "quantity": "3",
               "quantityUnit": "PCS"
               },
               "discount": 2500,
               "productUrl": "https://example.com/store/flipflops",
               "isReturn": false,
               "isShipping": false
            }
         ],
         "bottomLine": {
            "currency": "NOK",
            "tipAmount": 0,
            "posId": "pos_122",
            "paymentSources": {
               "giftCard": 0,
               "card": 0,
               "voucher": 0,
               "cash": 0
            },
            "barcode": {
               "format": "CODE 39",
               "data": "SC0527013501"
            },
            "receiptNumber": "0527013501"
         }
      },
      "reference":"acme-shop-123-1234589",
      "paymentDescription": "Invoice# 424243, due date: 01 Jan 2025",
      "returnUrl":"https://example.com/redirect?orderId=1512202",
      "userFlow":"WEB_REDIRECT",
   }'
   ```

</div>
</details>


## User flow alternatives

The `userFlow` parameter specifies how the API should handle the payment
and how the user experience will be.

| `userFlow`        | Description                                          |
| ----------------- | ---------------------------------------------------- |
| `WEB_REDIRECT`    | The normal flow for browser-based payment flows. If on a mobile device, the Vipps or MobilePay app will open. Otherwise, the [landing page](https://developer.vippsmobilepay.com/docs/knowledge-base/landing-page/) will open.    |
| `NATIVE_REDIRECT` | Automatic app-switch between the merchant's native app and the Vipps or MobilePay app. |
| `PUSH_MESSAGE`    | For payments initiated on a device other than the user's phone, the user gets a push message that opens the payment in the app. This is similar to [`skipLandingPage`](https://developer.vippsmobilepay.com/docs/knowledge-base/landing-page#skip-landing-page). |
| `QR`              | Returns a QR code that can be scanned to complete the payment. |

### WEB_REDIRECT

This is the default flow for wallet and card payments.

* Wallet flow:
  When the user is on a mobile device, redirect them to the Vipps or MobilePay app. From the desktop, open the
  [landing page](https://developer.vippsmobilepay.com/docs/knowledge-base/landing-page).

* Card flow:
  Whether the user is on a desktop or mobile device, open the card entry page. See
  [Card payments](https://developer.vippsmobilepay.com/docs/knowledge-base/payments#card-payments) for more information.

### NATIVE_REDIRECT

Applicable only for `WALLET` payments.

The `redirectUrl` will automatically open the Vipps or MobilePay app on mobile devices.

### PUSH_MESSAGE

Applicable only for `WALLET` payments.

For payments initiated on a device other than the user's phone, the user gets a push message that opens the payment in the app. This is similar to [`skipLandingPage`](https://developer.vippsmobilepay.com/docs/knowledge-base/landing-page#skip-landing-page).

If `userFlow` is `PUSH_MESSAGE`, a valid value for `customer.phoneNumber` is required, since there is no
way for the customer to enter the phone number manually.

### QR

Applicable only for `WALLET` payments.

This user flow is for customer-facing screens where payments can be initiated with the
[One-time payment QR](https://developer.vippsmobilepay.com/docs/APIs/qr-api/qr-api-guide/#one-time-payment-qr-codes).

### Receipt

Add a receipt to the payment. This includes the order Lines for the payment, which are identical to those referenced in the [Order Management](https://developer.vippsmobilepay.com/docs/APIs/order-management-api) API.

Applicable only for `WALLET` payments.
