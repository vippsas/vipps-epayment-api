<!-- START_METADATA
---
title: Migration
sidebar_label: Migration
id: migration
sidebar_position: 110
description: Guide for existing merchants who wish to migrate their integration to the ePayment API.
toc_min_heading_level: 2
toc_max_heading_level: 5
---

END_METADATA -->

# Migration fromt the eCom API to the ePayment API

The ePayment API expands upon the functionality of the eCom API and simplifies the existing flows. Merchants currently using the eCom API should find the ePayment API familiar and intuitive.

The ePayment API is **backwards compatible** with the eCom API. This means that payments initiated via the eCom API can be captured, refunded, cancelled, and retrieved using the ePayment API.

:::note
The ePayment API is backwards compatible with the eCom API. However, the eCom API is _not forwards compatible_ with the ePayment API. This means that **payments initiated with the ePayment API can not be modified or retrieved using the eCom API**.
:::

Merchants are advised to fully migrate over to the ePayment API. However, it is possible to migrate one endpoint at a time, _provided that the Create Payment endpoint is migrated last (see above note)_.

**Important:**
The ePayment API only offers “reserve capture”. There is no “direct capture”, as
in the eCom API. Read more about the benefits of "reserve capture":
[Reserve and capture](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/reserve-and-capture).

### Callbacks

For payment callbacks, you no longer have to submit the `callbackPrefix` as part of the Initiate Payment request. Instead, you can use the [Webhooks API](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/features/webhooks) to register URLs that will receive callbacks whenever various events occur for your payments.

The Webhooks API provides _guaranteed delivery_ callbacks. If the callback is not successfully received on your end, we will retry sending it for several days. In addition, you can now receive callbacks for _all_ adjustments to your payment. Head over to the [Webhooks documentation](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/features/webhooks) to read more.

### Payment flows

In the eCom API, merchants could choose between three flows by specifying the parameters `isApp` and `skipLandingPage`. These parameters were added to the original API over the years. The same functionality is available in the ePayment, but smarter: Instead of specifying the parameters, you now simply decide which flow you want through the `userFlow` property. Here's how the fields correspond to each other:

* `isApp: false` and `skipLandingPage: false` -> `WEB_REDIRECT`
* `isApp: true` and `skipLandingPage: false`  -> `NATIVE_REDIRECT`
* `skipLandingPage: true`                     -> `PUSH_MESSAGE`

The ePayment API also supports a new flow, [the `QR` flow](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/features/qr-payments). The QR flow provides you with a direct link to a one-time payment QR code that the user can scan and pay from their app.

### Renamed and altered fields

* `fallBack` -> Renamed `returnUrl`
* `scope` -> Moved inside the `profile` object
* `transactionText` -> Renamed `paymentDescription`
* `orderId` -> Renamed `reference`
* `phoneNumber`: Now requires the MSISDN format, where the country code is included. See the API specification, and [MSISDN](https://en.wikipedia.org/wiki/MSISDN).


### Payment Method

The ePayment API supports [free-standing card payments](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/features/free-standing-card-payments). Merchants are required to provide a value to the paymentMethod field to decide if this should be used.

:::note
The `WALLET` payment method means the user will use the Vipps app to pay.  The `CARD` payment method is intended for users who are not able to (or don't want to) use the app to pay.
:::
### Express Payments

Express payments on the ePayment API will be available soon.



### Partial Cancellations

Cancellation of partially captured payments is supported in the eCom API by setting the `shouldReleaseRemainingFunds` flag.  
In the ePayment API, this behavior works by default.


### Customer Present Payments

See [Customer-Present Payments](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/features/customer-present-payments).
