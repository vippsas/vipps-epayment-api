<!-- START_METADATA
---
title: Create the payment with the ePayment API
sidebar_label: Create
id: create
sidebar_position: 10
description: Create payment with the ePayment API.
---

END_METADATA -->

# Create payment

The first step in the payment flow is creating a payment by calling [CreatePayment](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments) endpoint. This endpoint supports card and wallet as payment methods and different user flows for each.

```mermaid
flowchart TD
    Payment --> Wallet[fa:fa-mobile paymentMethod: Wallet]
    Wallet --> WEB_REDIRECT[fa:fa-desktop userFlow: WEB_REDIRECT]
    Wallet --> NATIVE_REDIRECT[fa:fa-mobile userFlow: NATIVE_REDIRECT]
    Wallet --> PUSH_MESSAGE[fa:fa-bell userFlow: PUSH_MESSAGE]
    Wallet --> QR[fa:fa-qrcode userFlow: QR]
    Payment --> Card[fa:fa-credit-card paymentMethod:Card]
    Card --> CARD_WEB_REDIRECT[fa:fa-desktop userFlow: WEB_REDIRECT]
```

`paymentMethod.type` in the request determines the type of payment. Allowed values are `CARD` and `WALLET`. Please note that card payment is not available in our test environment, but works in production.

## User flow alternatives

### WEB_REDIRECT

Default flow for:

- wallet payments
    - Opening the Vipps Landing Page on desktop, and automatic redirect to Vipps on mobile devices. More information at [Vipps landing page](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/vipps-landing-page)
- card payments
    - Opening the Vipps card entry page on both desktop/mobile. More information at [Card payments](https://developer.vippsmobilepay.com/docs/APIs/checkout-api/vipps-checkout-api-faq#card-payments)


### NATIVE_REDIRECT

Applicable only for Wallet payments.
The given `redirectUrl` will automatically open the Vipps app on mobile devices.


### PUSH_MESSAGE

Applicable only for Wallet payments. This will skip the Vipps landing page and is only allowed if user is not starting the payment from own device (e.g., from a Point Of Sale device, automates and similar).
If userFlow is `PUSH_MESSAGE`, a valid value for `$.customer.phoneNumber` is required.

### QR

Applicable only for Wallet payments. For customer facing screens where payment can be initiated with [Vipps One Time Payment QR](https://developer.vippsmobilepay.com/docs/APIs/qr-api/vipps-qr-one-time-payment-api-howitworks).
