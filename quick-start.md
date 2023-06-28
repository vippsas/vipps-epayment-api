---
title: Quick start
sidebar_label: Quick start
id: quick-start
sidebar_position: 10
description: Quick steps for getting started with the ePayment API.
toc_min_heading_level: 2
toc_max_heading_level: 5
pagination_next: null
pagination_prev: null
---

import ApiSchema from '@theme/ApiSchema';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Quick start

## Before you begin

This document covers the quick steps for getting started with the ePayment API.
You must have already signed up as an organization with Vipps MobilePay and have
your test credentials from the merchant portal, as described in the
[Vipps Quick start guide](https://developer.vippsmobilepay.com/docs/vipps-developers/getting-started).

**Important:** The examples use standard example values that you must change to
use *your* values. This includes API keys, HTTP headers, reference, etc.

## Your first Vipps Payment

### Step 1 - Setup

<Tabs
defaultValue="postman"
groupId="sdk-choice"
values={[
{label: 'Postman', value: 'postman'},
{label: 'curl', value: 'curl'},
]}>
<TabItem value="postman">

Save the following files to your computer:

* [ePayment API Postman collection](/tools/vipps-epayment-api-postman-collection.json)
* [Global Postman environment](https://github.com/vippsas/vipps-developers/blob/master/tools/vipps-api-global-postman-environment.json)

Then, import the Postman files

1. In Postman, click *Import* in the upper-left corner.
1. In the dialog that opens, with *File* selected, click *Upload Files*.
1. Select the two files you have just downloaded and click *Import*.

Lastly, tweak the Postman environment with your own secrets

1. Click the down arrow, next to the "eye" icon in the top-right corner, and select the environment you have imported.
2. Click the "eye" icon and, in the dropdown window, click `Edit` in the top-right corner.
3. Fill in the `Current Value` for the following fields to get started.
   For the first keys, go to *Vipps Portal* > *Utvikler* ->  *Test Keys*.
    * `client_id` - Merchant key is required for getting the access token.
    * `client_secret` - Merchant key is required for getting the access token.
    * `Ocp-Apim-Subscription-Key` - Merchant subscription key.
    * `merchantSerialNumber` - Merchant ID.
    * `internationalMobileNumber` - The MSISDN for the test app profile you have received or registered. This is your test mobile number *including* country code.

</TabItem>
<TabItem value="curl">

No setup needed :)

</TabItem>
<TabItem value="csharp">

Install the Vipps .NET SDK to your project

```csharp
dotnet add package vipps.net
```

</TabItem>
</Tabs>

### Step 2 - Authentication

For all the following, you will need an `access_token` from the
[Access token API](https://developer.vippsmobilepay.com/docs/APIs/access-token-api):
[`POST:/accesstoken/get`][access-token-endpoint]
This provides you with access to the API.

<Tabs
defaultValue="postman"
groupId="sdk-choice"
values={[
{label: 'Postman', value: 'postman'},
{label: 'curl', value: 'curl'},
]}>
<TabItem value="postman">

```bash
Send request Get Access Token
```

</TabItem>
<TabItem value="curl">

```bash
curl https://apitest.vipps.no/accessToken/get \
-H "client_id: YOUR-CLIENT-ID" \
-H "client_secret: YOUR-CLIENT-SECRET" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-H "Merchant-Serial-Number: 123456" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-H "Vipps-System-Plugin-Name: acme-webshop" \
-H "Vipps-System-Plugin-Version: 4.5.6" \
-X POST \
--data ''
```

</TabItem>
<TabItem value="csharp">

The [.NET SDK](https://developer.vippsmobilepay.com/docs/SDKs/dotnet-sdk/) will handle access token fetching for you, all you need to do is to add the following configuration code

```csharp
var vippsConfigurationOptions = new VippsConfigurationOptions
{
    ClientId = YOUR-CLIENT-ID,
    ClientSecret = YOUR-CLIENT-SECRET,
    MerchantSerialNumber = YOUR-MERCHANT-SERIAL-NUMBER,
    SubscriptionKey = YOUR-SUBSCRIPTION-KEY,
    UseTestMode = true // TODO?
};

VippsConfiguration.ConfigureVipps(vippsConfigurationOptions);
```

</TabItem>
</Tabs>

The property `access_token` should be used for all other API requests in the `Authorization` header as the Bearer token.

### Step 3 - A simple ePayment payment with web redirect

Initiate a payment with: [`POST:/payments`][create-payment-endpoint].
In this example, we use the default user flow, `WEB_REDIRECT`.
This provides you with a link you can click to go to the landing page.
When your test mobile number (in MSISDN format)
is provided in `phoneNumber`, it will be pre-filled in the form.

<Tabs
defaultValue="postman"
groupId="sdk-choice"
values={[
{label: 'Postman', value: 'postman'},
{label: 'curl', value: 'curl'},
]}>
<TabItem value="postman">

```bash
Send request Create Payment - Web redirect
```

</TabItem>
<TabItem value="curl">

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
-H "Idempotency-Key: 49ca711a-acee-4d01-993b-9487112e1def" \
-X POST \
-d '{
  "amount": {
    "currency": "NOK",
    "value": 1000
  },
  "paymentMethod": {
    "type": "WALLET"
  },
  "customer": {
    "phoneNumber": "4791234567"
  },
  "reference": "acme-shop-123-order123abc",
  "returnUrl": "https://yourwebsite.come/redirect?reference=abcc123",
  "userFlow": "WEB_REDIRECT",
  "paymentDescription": "One pair of Vipps socks"
}'
```

</TabItem>
<TabItem value="csharp">

```csharp
var reference = Guid.NewGuid().ToString();
var customerPhoneNumber = "4791234567";
var createPaymentRequest = new CreatePaymentRequest
    {
        Amount = new Amount
        {
            Currency = Currency.NOK,
            Value = 1000 // 1000 øre = 10 KR
        },
        PaymentMethod = new PaymentMethod { Type = PaymentMethodType.WALLET },
        UserFlow = CreatePaymentRequestUserFlow.WEB_REDIRECT,
        Reference = reference,
        PaymentDescription = "Two pairs of socks and one coffee",
        ReturnUrl = $"https://no.where.com/{reference}",
        Customer = new Customer { PhoneNumber = customerPhoneNumber }
    };
var createPaymentResult = await EpaymentService.CreatePayment(createPaymentRequest);
```

</TabItem>
</Tabs>

### Step 4 - Completing the payment

*Ctrl+click* (*Command-click* on macOS) on the link that appears, and it will take you to the Vipps landing page.
The phone number of your test user should already be filled in, so you only have to click "Next".
You will be presented with the payment in the Vipps app, where you can complete or reject the payment.
Once you have acted upon the payment you will be redirected back to the specified `returnUrl` under a "best effort" policy.

:::note
We cannot guarantee the user will be redirected back to the same browser or session, or that they will at all be redirected back. User interaction can be unpredictable, and the user may choose to fully close the Vipps app or browser.
:::

### Step 5 - Getting the status of the payment

To receive the result of the user action you may poll the status of the payment via the
[`GET:/payments/{reference}`][get-payment-endpoint].

<Tabs
defaultValue="postman"
groupId="sdk-choice"
values={[
{label: 'Postman', value: 'postman'},
{label: 'curl', value: 'curl'},
]}>
<TabItem value="postman">

```bash
Send request Get payment
```

</TabItem>
<TabItem value="curl">

```bash
curl https://apitest.vipps.no/epayment/v1/payments/UNIQUE-PAYMENT-REFERENCE \
-H "Content-Type: application/json" \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <truncated>" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Merchant-Serial-Number: 123456" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-H "Vipps-System-Plugin-Name: acme-webshop" \
-H "Vipps-System-Plugin-Version: 4.5.6" \
-H "Idempotency-Key: 49ca711a-acee-4d01-993b-9487112e1def" \
-X GET
```

</TabItem>
<TabItem value="csharp">

```csharp
var payment = await EpaymentService.GetPayment(reference);
```

</TabItem>
</Tabs>

To verify that a payment has been authorized by the user, check that the `state`
property is marked `AUTHORIZED`. If the user has instead chosen to reject the
payment or chosen to click `cancel` on the landing page or in the Vipps App,
the `state` property will be marked `ABORTED`. If the user did not act within
the payment expiration time, the `state` property will be marked `EXPIRED`.

For more details of the lifecycle of the payment session the
[`GET:/payments/{reference}/events`][get-payment-event-log-endpoint] endpoint.

<Tabs
defaultValue="postman"
groupId="sdk-choice"
values={[
{label: 'Postman', value: 'postman'},
{label: 'curl', value: 'curl'},
]}>
<TabItem value="postman">

```bash
Send request Get payment event log
```

</TabItem>
<TabItem value="curl">

```bash
curl https://apitest.vipps.no/epayment/v1/payments/UNIQUE-PAYMENT-REFERENCE/events \
-H "Content-Type: application/json" \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <truncated>" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Merchant-Serial-Number: 123456" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-H "Vipps-System-Plugin-Name: acme-webshop" \
-H "Vipps-System-Plugin-Version: 4.5.6" \
-H "Idempotency-Key: 49ca711a-acee-4d01-993b-9487112e1def" \
-X GET
```

</TabItem>
<TabItem value="csharp">

```csharp
var paymentEventLog = await EpaymentService.GetPaymentEventLog(reference);
```

</TabItem>
</Tabs>

### Step 6 - Capture the payment

After the goods or services have been delivered, you can capture the authorized
amount either partially or fully:
[`POST:/payments/{reference}/capture`][capture-payment-endpoint].

<Tabs
defaultValue="postman"
groupId="sdk-choice"
values={[
{label: 'Postman', value: 'postman'},
{label: 'curl', value: 'curl'},
]}>
<TabItem value="postman">

```bash
Send request Capture payment
```

</TabItem>
<TabItem value="curl">

```bash
curl https://apitest.vipps.no/epayment/v1/payments/UNIQUE-PAYMENT-REFERENCE/capture \
-H "Content-Type: application/json" \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <truncated>" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Merchant-Serial-Number: 123456" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-H "Vipps-System-Plugin-Name: acme-webshop" \
-H "Vipps-System-Plugin-Version: 4.5.6" \
-H "Idempotency-Key: 49ca711a-acee-4d01-993b-9487112e1def" \
-X POST \
-d '{
  "modificationAmount": {
    "currency": "NOK",
    "value": 1000
  }
}'
```

</TabItem>
<TabItem value="csharp">

```csharp
var captureAmount = new Amount() { Value = 1000, Currency = Currency.NOK };
var captureResult = await EpaymentService.CapturePayment(
reference,
new CaptureModificationRequest { ModificationAmount = captureAmount });
```

</TabItem>
</Tabs>

See
[Common topics: Capture](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/reserve-and-capture#capture)
for more details about the types of captures.

### (Optional) Step 7 - Refund the payment

To refund the captured amount, either partially or fully:
[`POST:/payments/{reference}/refund`][refund-payment-endpoint].

<Tabs
defaultValue="postman"
groupId="sdk-choice"
values={[
{label: 'Postman', value: 'postman'},
{label: 'curl', value: 'curl'},
]}>
<TabItem value="postman">

```bash
Send request Refund payment
```

</TabItem>
<TabItem value="curl">

```bash
curl https://apitest.vipps.no/epayment/v1/payments/UNIQUE-PAYMENT-REFERENCE/refund \
-H "Content-Type: application/json" \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <truncated>" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Merchant-Serial-Number: 123456" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-H "Vipps-System-Plugin-Name: acme-webshop" \
-H "Vipps-System-Plugin-Version: 4.5.6" \
-H "Idempotency-Key: 49ca711a-acee-4d01-993b-9487112e1def" \
-X POST \
-d '{
  "modificationAmount": {
    "currency": "NOK",
    "value": 1000
  }
}'
```

</TabItem>
<TabItem value="csharp">

```csharp
var refundAmount = new Amount() { Value = 1000, Currency = Currency.NOK };
var refundResult = await EpaymentService.RefundPayment(
reference,
new RefundModificationRequest { ModificationAmount = refundAmount });
```

</TabItem>
</Tabs>

See
[Common topics: refund](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/refund)
for more details about refunds.

### (Optional) Step 8 - Cancel the payment

To cancel the payment, either fully or after a partial capture:
[`POST:/payments/{reference}/cancel`][cancel-payment-endpoint].

<Tabs
defaultValue="postman"
groupId="sdk-choice"
values={[
{label: 'Postman', value: 'postman'},
{label: 'curl', value: 'curl'},
]}>
<TabItem value="postman">

```bash
Send request Cancel payment
```

</TabItem>
<TabItem value="curl">

```bash
curl https://apitest.vipps.no/epayment/v1/payments/UNIQUE-PAYMENT-REFERENCE/cancel \
-H "Content-Type: application/json" \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <truncated>" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Merchant-Serial-Number: 123456" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-H "Vipps-System-Plugin-Name: acme-webshop" \
-H "Vipps-System-Plugin-Version: 4.5.6" \
-H "Idempotency-Key: 49ca711a-acee-4d01-993b-9487112e1def" \
-X POST
```

</TabItem>
<TabItem value="csharp">

``` csharp
var cancelResult = await EpaymentService.CancelPayment(reference);
```

</TabItem>
</Tabs>

See
[Common topics: Cancel](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/cancel)
for more details about cancel.

## Next Steps

Now that you have completed your first payment,
read further to see the full range of possibilities within the Vipps ePayment API.

* [Capture the payment](operations/capture.md)
* [Payment modification, how to use cancel, capture and refund?](operations/README.md)
* [Using Vipps ePayment API in a shopper present context](features/customer-present-payments.md)
* [Profile sharing, requesting the users personal information](features/profile-sharing.md)

[access-token-endpoint]: https://developer.vippsmobilepay.com/api/access-token#tag/Authorization-Service/operation/fetchAuthorizationTokenUsingPost
[create-payment-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments/operation/createPayment
[get-payment-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/QueryPayments/operation/getPayment
[get-payment-event-log-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/QueryPayments/operation/getPaymentEventLog
[cancel-payment-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/AdjustPayments/operation/cancelPayment
[capture-payment-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/AdjustPayments/operation/capturePayment
[refund-payment-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/AdjustPayments/operation/refundPayment
[adjust-authorization-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/AdjustPayments/operation/adjustAuthorization
[force-approve-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/ForceApprove/operation/forceApprove
