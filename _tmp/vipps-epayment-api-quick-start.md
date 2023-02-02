<!-- START_METADATA
---
title: Quick start
sidebar_position: 10
draft: true
---
END_METADATA -->

# Quick start

Use the ePayment API to create payments using each of the following user flows:

* WEB_REDIRECT - Open the [Vipps landing page](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/common-topics/vipps-landing-page).
* NATIVE_REDIRECT - Open the Vipps app on the user's mobile phone.
* PUSH_MESSAGE - Cause a notification from the user's Vipps app.
* QR - Generate a QR code that a user can scan to send the request to their Vipps app.

You can also get payment details, get user info, capture payments, and refund payments.
Request user consent to information and review information.

<!-- START_COMMENT -->

ℹ️ Please use the new documentation:
[Vipps Technical Documentation](https://vippsas.github.io/vipps-developer-docs/).

## Table of Contents

* [Postman](#postman)
  * [Prerequisites](#prerequisites)
  * [Step 1: Get the Vipps Postman collection and environment](#step-1-get-the-vipps-postman-collection-and-environment)
  * [Step 2: Import the Vipps Postman files](#step-2-import-the-vipps-postman-files)
  * [Step 3: Set up Postman environment](#step-3-set-up-postman-environment)
* [Make API calls](#make-api-calls)
  * [A simple ePayment payment with web redirect](#a-simple-epayment-payment-with-web-redirect)
  * [An ePayment payment which provides a QR code](#an-epayment-payment-which-provides-a-qr-code)
  * [An ePayment payment which causes a push request](#an-epayment-payment-which-causes-a-push-request)
  * [An ePayment payment which causes a native redirect](#an-epayment-payment-which-causes-a-native-redirect)
  * [Getting access to user info](#getting-access-to-user-info)
  * [Test with Force Approve](#test-with-force-approve)

<!-- END_COMMENT -->

## Postman

### Prerequisites

Review
[Vipps quick start guides](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/quick-start-guides) for information about getting your test environment set up.

### Step 1: Get the Vipps Postman collection and environment

Save the following files to your computer:

* [Vipps ePayment API Postman collection](tools/vipps-epayment-api-postman-collection.json)
* [Vipps API Global Postman environment](https://raw.githubusercontent.com/vippsas/vipps-developers/master/tools/vipps-api-global-postman-environment.json)

### Step 2: Import the Vipps Postman files

1. In Postman, click *Import* in the upper-left corner.
1. In the dialog that opens, with *File* selected, click *Upload Files*.
1. Select the two files you have just downloaded and click *Import*.

### Step 3: Set up Postman environment

1. Click the down arrow, next to the "eye" icon in the top-right corner, and select the environment you have imported.
2. Click the "eye" icon and, in the dropdown window, click `Edit` in the top-right corner.
3. Fill in the `Current Value` for the following fields to get started.
   For the first keys, go to *Vipps Portal* > *Utvikler* ->  *Test Keys*.
   * `client_id` - Merchant key is required for getting the access token.
   * `client_secret` - Merchant key is required for getting the access token.
   * `Ocp-Apim-Subscription-Key` - Merchant subscription key.
   * `merchantSerialNumber` - Merchant id.
   * `internationalMobileNumber` - The MSISDN for the test app profile you have received or registered. This is your test mobile number *including* country code.

You can update any of the other environment variables. Be aware of this:

* All currency amounts are specified in minor units (e.g., øre) and the minimum value is 100 minor units. For example, 10 nok = 1000 øre.
* Most URLs must be `https`.

## Make API calls

For all of the following, you will start by sending request `Get Access Token` which uses
[Get Access Token][access-token-endpoint].
This provides you with access to the API.

The access token is valid for 1 hour in the test environment
and 24 hours in the production environment.
See the
[API reference][epayment-api-reference-url]
for details about the calls.

### A simple ePayment payment with web redirect

This provides you with a link you can click to go to the landing page. When your test mobile number (with country code)
is provided, it will be prefilled in the form.

1. Send request `Get Access Token`. This provides you with access to the API.

1. Send request `Create Payment - Web redirect`. This is to demonstrate a simple payment by using
   [`POST:/epayment/v1/payments`][create-payment-endpoint].

   For this payment type, `userFlow` is set to `WEB_REDIRECT`.

   The `reference` and `vippsLandingPageUrl` variables are now in the environment
   of this Postman example and can be used for subsequent calls relating to this purchase.

   The response will be a URL to the Vipps landing page.
   *Ctrl+click* (*Command-click* on macOS) on the link that appears and it will take
   you to the Vipps landing page.
   The phone number of your test user should already be filled in, so you only have to click "Next".

   You have now confirmed the payment in Vipps, setting the payment status to reserved.

1. Send request `Get payment` for information about this payment by using
   [`GET:/epayment/v1/payments`][get-payment-endpoint].
   The `reference` is set in the body of the request. You will see the details appear in the lower pane.

1. Send request `Get payment event log` for information about this payment by using
   [`GET:/epayment/v1/payments/{{reference}}/events`][get-payment-event-log-endpoint].
   You will see the details appear in the lower pane.

1. Send request `Capture payment` to capture this payment with
   [`POST:/epayment/v1/payments/{{reference}}/capture`][capture-payment-endpoint].

1. Send request `Refund payment` to refund this payment with
   [`POST:/epayment/v1/payments/{{reference}}/refund`][refund-payment-endpoint].

### An ePayment payment which provides a QR code

This generates a QR code that a user can scan to initiate a Vipps authorization on their mobile phone.

1. Send request `Initiate Payment - QR`. This demonstrates the type
   of payment where you can generate a QR.

   For this payment type, `userFlow` is set to `QR`.
   You can optionally specify the `qrFormat` settings of format and size.

   The `reference` is set in the environment for use with subsequent calls.

2. Send request `Get Payment Details` for information about this payment.

3. This time, instead of capturing the order, cancel it. Send request `Cancel Payment`
   to cancel this payment with
   [`POST:/epayment/v1/payments/{{reference}}/cancel`][cancel-payment-endpoint].

### An ePayment payment which causes a push request

1. Send request `Initiate Payment - Push request`. This demonstrates the type
   of payment where you cause a notification from the Vipps app on the user's mobile phone.

   You must provide your test mobile number *and*
   your sale unit must be configured for *skip landing page*.

   For this payment type, `userFlow` is set to `PUSH_MESSAGE`.

   The `reference` is set in the environment for use with subsequent calls.

1. Send request `Get payment` for information about this payment.

### An ePayment payment which causes a native redirect

1. Send request `Initiate Payment - Native redirect`. This demonstrates opening up the
   Vipps app with the authorization page. The URL that is provided must be clicked on the mobile phone.
   This means that, to test it, you need to send the URL to your phone and click it there.
   That should open the Vipps app.

   For this payment type, `userFlow` is set to `NATIVE_REDIRECT`.

   The `reference` is set in the environment for use with subsequent calls.


### Getting access to user info

1. Send request `Initiate Payment - User info`. Provide the `scope` object in the
   [`POST:/epayment/v1/payments`][create-payment-endpoint]
   call. This contains the information types that you want access to, separated
   by spaces (e.g., "name address email phoneNumber birthDate").

   This example uses, `userFlow` is set to `WEB_REDIRECT`.

   Ctrl+click on the link that appears and complete the authorization.

1. Send request `Get payment` for information about this payment by using
   [`GET:/epayment/v1/payments`][get-payment-endpoint].
   The `reference` is set in the body of the request.

   The user identifier, `sub`, is retrieved from the response and set as a variable.

1. Send request `Get Userinfo`. This uses
   [`GET:/vipps-userinfo-api/userinfo/{sub}`][get-user-info-endpoint]
   with the `sub` variable from the previous call.


### Test with Force Approve

You can use the
[`epayment/v1/test/payments/{{reference}}/approve`][force-approve-endpoint]
endpoint
to approve an ePayment API payment without signing in to the Vipps MT app.
The endpoint is only available in our test environment.

**Important:** All test users must manually approve at least one payment in
Vipps (using the app) before "force approve" can be used for that user.
If this has not been done, you will get an error.
This is because the user needs to be registered as
"bankID verified" in the backend, and this is happens automatically in
the test environment when using Vipps (the app), but not with "force approve".



[epayment-api-reference-url]: https://vippsas.github.io/vipps-developer-docs/api/epayment
[create-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/CreatePayments/operation/createPayment
[get-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/QueryPayments/operation/getPayment
[get-payment-event-log-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/QueryPayments/operation/getPaymentEventLog
[cancel-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/cancelPayment
[capture-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/capturePayment
[refund-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/refundPayment
[adjust-authorization-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/adjustAuthorization
[force-approve-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/ForceApprove/operation/forceApprove
[get-user-info-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/ecom#tag/Vipps-Userinfo-API/operation/getUserinfo
[access-token-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/access-token#tag/Authorization-Service/operation/fetchAuthorizationTokenUsingPost
[portal-url]: https://portal.vipps.no