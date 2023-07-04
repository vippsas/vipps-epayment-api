---
title: Migration
sidebar_label: Migration
id: migration
sidebar_position: 110
description: Guide for existing merchants who wish to migrate their integration to the ePayment API.
toc_min_heading_level: 2
toc_max_heading_level: 5
---


# Migration from the eCom API to the ePayment API

The ePayment API expands upon the functionality of the eCom API and simplifies the existing flows.
Merchants currently using the eCom API should find the ePayment API familiar and intuitive.

The ePayment API is *backwards compatible* with the eCom API. This means that payments initiated
via the eCom API can be captured, refunded, cancelled, and retrieved using the ePayment API.

:::note
The ePayment API is backwards compatible with the eCom API. However, the eCom API is *not forwards compatible* with the ePayment API. This means that *payments initiated with the ePayment API can not be modified or retrieved using the eCom API*.
:::

Merchants are advised to fully migrate over to the ePayment API. However, it is possible to migrate one endpoint at a time, *provided that the Create Payment endpoint is migrated last (see above note)*.

See
[Can payments be "mixed and matched" between the eCom API and the ePayment API?](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/faq/#can-payments-be-mixed-and-matched-between-the-ecom-api-and-the-epayment-api)

**Important:**
The ePayment API only offers “reserve capture”. There is no “direct capture”, as
in the eCom API. Read more about the benefits of "reserve capture":
[Reserve and capture](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/reserve-and-capture).

## Callbacks

For payment callbacks, you no longer have to submit the `callbackPrefix` as part of the Initiate Payment request
Instead, you can use the
[Webhooks API](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/features/webhooks)
to register URLs that will receive callbacks whenever various events occur for your payments.

**Please note:** The Webhooks API provides *guaranteed delivery*: If the callback is not successful
(we do not get the expected response from you), we will retry sending it for several days.
In addition, you can now receive callbacks for *all* adjustments to your payment.

## Payment flows

In the eCom API, merchants could choose between three flows by specifying the parameters
[`isApp`](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/isApp/)
and
[`skipLandingPage`](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/vipps-landing-page/#skip-landing-page).

These parameters were added to the original API over the years. The same functionality is available in ePayment,
but smarter: Instead of specifying the parameters, you now simply decide which flow you want through the
`userFlow` property. Here's how the fields correspond to each other:

* `isApp: false` and `skipLandingPage: false` -> `WEB_REDIRECT`
* `isApp: true` and `skipLandingPage: false`  -> `NATIVE_REDIRECT`
* `skipLandingPage: true`                     -> `PUSH_MESSAGE`

The ePayment API also supports a new flow:
[the `QR` flow](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/features/qr-payments).
The QR flow provides you with a direct link to a one-time payment QR code that the user can scan and pay from their app.

## Renamed and altered fields

* `fallBack` -> Renamed `returnUrl`
* `scope` -> Moved inside the `profile` object
* `transactionText` -> Renamed `paymentDescription`
* `orderId` -> Renamed `reference`
* `phoneNumber`: Now requires the MSISDN format, where the country code is included. See the API specification, and [MSISDN](https://en.wikipedia.org/wiki/MSISDN).

## Payment Method

The ePayment API supports [freestanding card payments](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/features/free-standing-card-payments). Merchants are required to provide a value to the `paymentMethod` field to decide if this should be used.

:::note
The `WALLET` payment method means the user will use the Vipps app to pay.  The `CARD` payment method is intended for users who are not able to (or don't want to) use the app to pay.
:::

## Express Payments

See:
[Is Express Checkout available in the ePayment API?](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/faq/#is-express-checkout-available-in-the-epayment-api)

## Partial Cancellations

Cancellation of partially captured payments is supported in the eCom API by setting the `shouldReleaseRemainingFunds` flag
in the
[`PUT:/ecomm/v2/payments/{orderId}/cancel`](https://developer.vippsmobilepay.com/api/ecom/#tag/Vipps-eCom-API/operation/cancelPaymentRequestUsingPUT)
request.

In the ePayment API, this behavior is the default behavior for the
[`POST:/epayment/v1/payments/{reference}/cancel`](https://developer.vippsmobilepay.com/api/epayment/#tag/AdjustPayments/operation/cancelPayment)
request, and there is no need to do anything extra.

## Customer Present Payments

See [Customer-Present Payments](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/features/customer-present-payments).
