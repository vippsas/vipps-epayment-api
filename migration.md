<!-- START_METADATA
---
title: Migration
sidebar_label: Migration
id: migration
sidebar_position: 11
description: Guide for existing merchants who wish to migrate their integration to the ePayment API.
toc_min_heading_level: 2
toc_max_heading_level: 5
pagination_next: null
pagination_prev: null
---

END_METADATA -->

# Migration

## Why migrate to ePayment API?

The Vipps MobilePay ePayment API serves as the replacement for both the existing [eCom API](https://vippsas.github.io/vipps-developer-docs/docs/APIs/ecom-api) and the [App Payments API](https://developer.mobilepay.dk/docs/app-payments). The ePayment API will receive future updates and features, while the other APIs will only receive maintenance support. Merchants are therefore advised to migrate to the ePayment API.

Features of the ePayment API include:
* [Profile Sharing](https://vippsas.github.io/vipps-developer-docs/docs/APIs/epayment-api/features/profile-sharing)
* [Long-Living transactions](https://vippsas.github.io/vipps-developer-docs/docs/APIs/epayment-api/features/long-living-payments)
* [Free Standing Card Payments](https://vippsas.github.io/vipps-developer-docs/docs/APIs/epayment-api/features/free-standing-card-payments)
* [QR Payments](https://vippsas.github.io/vipps-developer-docs/docs/APIs/epayment-api/features/qr-payments)
* [Webhooks](https://vippsas.github.io/vipps-developer-docs/docs/APIs/epayment-api/features/webhooks)


## Migrating from the eCom API

The ePayment API expands upon the functionality of the eCom API and simplifies the existing flows. Merchants currently using the eCom API should find the ePayment API familiar and intuitive.

The ePayment API is **backwards compatible** with the eCom API. This means that payments initiated via the eCom API can be captured, refunded, cancelled, and retrieved using the ePayment API.

:::note
The ePayment API is backwards compatible with the eCom API. However, the eCom API is _not forwards compatible_ with the ePayment API. This means that **payments initiated with the ePayment API can not be modified or retrieved using the eCom API**.
:::

Merchants are advised to fully migrate over to the ePayment API. However, it is possible to migrate one endpoint at a time, _provided that the Create Payment endpoint is migrated last (see above note)_.

### Callbacks

For payment callbacks, you no longer have to submit the `CallbackPrefix` as part of the Initiate Payment request. Instead, you can use the [Webhooks API](https://vippsas.github.io/vipps-developer-docs/docs/APIs/epayment-api/features/webhooks) to register URLs that will receive callbacks whenever various events occur for your payments.

The Webhooks API provides _Guaranteed Delivery_ callbacks. If the callback is not successfully received on your end, we will retry sending it for several days. In addition, you can now receive callbacks for _all_ adjustments to your payment. Head over to the [Webhooks documentation](https://vippsas.github.io/vipps-developer-docs/docs/APIs/epayment-api/features/webhooks) to read more.

### Payment flows

In the eCom API, merchants could choose between three flows by setting the flags `isApp` and `skipLandingPage`. These flows exist in ePayment as well: Instead of setting the flags, you now simply decide which flow you want through the `userFlow` property. Here's how the fields correspond to each other:

* `isApp: false` and `skipLandingPage: false` -> `WEB_REDIRECT`
* `isApp: true` and `skipLandingPage: false`  -> `NATIVE_REDIRECT`
* `skipLandingPage: true`                     -> `PUSH_MESSAGE`

The ePayment API also supports a new flow, [the `QR` flow](https://vippsas.github.io/vipps-developer-docs/docs/APIs/epayment-api/features/qr-payments).

### Renamed and altered fields

* `fallBack` -> Renamed `returnUrl`
* `scope` -> Moved inside `profile` object
* `transactionText` -> Renamed `paymentDescription`
* `orderId` -> Renamed `reference`
* `phoneNumber`: Now requires country code/MSISDN format


### Payment Method

The ePayment API supports [Free standing Card Payments](https://vippsas.github.io/vipps-developer-docs/docs/APIs/epayment-api/features/free-standing-card-payments). Merchants are required to provide a value to the paymentMethod field to decide if this should be used.

:::note
The Wallet PaymentMethod means the user will use the Vipps app to pay. _The Card PaymentMethod is intended for users who don't want to use the app to pay._
:::
### Express Payments

Express payments on the ePayment API will be available soon.


### Direct Capture

Merchants that are configured for Direct Capture now need to set the `directCapture` flag to `true`.


### Partial Cancellations

Cancellation of partially captured payments is supported in the eCom API by setting the `shouldReleaseRemainingFunds` flag.  
In the ePayment API, this behavior works by default.


### Customer Present Payments

See [Customer-Present Payments](https://vippsas.github.io/vipps-developer-docs/docs/APIs/epayment-api/features/customer-present-payments).