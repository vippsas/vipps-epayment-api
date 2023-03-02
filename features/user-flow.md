<!-- START_METADATA
---
title: User flow alternatives
hide_table_of_contents: true
pagination_next: null
pagination_prev: APIs/epayment-api/getting-started
---
END_METADATA -->

# User flow alternatives

The different flows for bringing the user to the Vipps Wallet payment confirmation screen.
Flag payment with `"userFlow": "WEB_REDIRECT"`.

### WEB_REDIRECT

Default flow for payments, opening the Vipps Landing Page on desktop, and automatic redirect to Vipps on mobile devices. 
Must be used when payment type is `CARD`.

### PUSH_MESSAGE

This will skip the Vipps landing page and is only allowed if user is not starting the payment from own device (e.g., from a Point Of Sale device, automates and similar). 
If userFlow is `PUSH_MESSAGE`, a valid value for `$.customer.phoneNumber` is required.

### NATIVE_REDIRECT

Flow for opening a native iOS or Android app with deeplink to the app.

### QR

For customer facing screens where payment can be initiated with [Vipps One Time Payment QR](https://vippsas.github.io/vipps-developer-docs/docs/APIs/qr-api/vipps-qr-one-time-payment-api-howitworks).

