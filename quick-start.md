---
title:  Quick start for the ePayment API
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

The provided example values in this guide must be changed with the values for your test sales unit and user.
This applies for API keys, HTTP headers, reference, phone number, etc.
Note that any currency amount must be an Integer value minimum 100 in øre.

## Your first Payment

### Step 1 - Setup

You must have already signed up as an organization with Vipps MobilePay and have
your test credentials from the merchant portal.

You will need the following values, as described in the
[Getting started guide](https://developer.vippsmobilepay.com/docs/getting-started):

* `client_id` - Client_id for a test sales unit.
* `client_secret` - Client_id for a test sales unit.
* `Ocp-Apim-Subscription-Key` - Subscription key for a test sales unit.
* `merchantSerialNumber` - The unique ID for a test sales unit.
* `internationalMobileNumber` - The MSISDN for the test app profile you have received or registered. This is your test mobile number *including* country code.

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
]}>
<TabItem value="postman">

In Postman, import the following files:

* [ePayment API Postman collection](/tools/vipps-epayment-api-postman-collection.json)
* [Global Postman environment](https://github.com/vippsas/vipps-developers/blob/master/tools/vipps-api-global-postman-environment.json)

🔥 **To reduce risk of exposure, never store production keys in Postman or any similar tools.** 🔥

Update the *Current Value* field in your Postman environment with your **Merchant Test** keys.
Use *Current Value* field for added security, as these values are not synced to the cloud.


</TabItem>
<TabItem value="curl">

No additional setup needed :)

</TabItem>
<TabItem value="csharp">

Install the .NET SDK to your project

```csharp
dotnet add package vipps.net
```

</TabItem>
</Tabs>

### Step 2 - Authentication

For all the following, you will need an `access_token` from the
[Access token API](https://developer.vippsmobilepay.com/docs/APIs/access-token-api):
[`POST:/accesstoken/get`][access-token-endpoint].
This provides you with access to the API.

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
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
-H "Merchant-Serial-Number: YOUR-MSN" \
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

### Step 3 - Request a simple payment

Initiate a payment with: [`POST:/payments`][create-payment-endpoint].
In this example, we use the default user flow, `WEB_REDIRECT`.
This provides you with a link you can click to go to the
[landing page](https://developer.vippsmobilepay.com/docs/common-topics/landing-page/).
When your test mobile number (in MSISDN format)
is provided in `phoneNumber`, it will be pre-filled in the form.

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
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
-H "Merchant-Serial-Number: YOUR-MSN" \
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
  "paymentDescription": "One pair of socks"
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

### Step 4 - Complete the payment

Open the `redirectUrl` link that is returned, and it will take you to the
[landing page](https://developer.vippsmobilepay.com/docs/common-topics/landing-page/).
The phone number of your test user should already be filled in, so you only have to click *Next*.

You will be presented with the payment in the app, where you can complete the payment and be directed to the specified `returnUrl` under a "best effort" policy.

:::note
We cannot guarantee the user will be redirected back to the same browser or session, or that they will at all be redirected back. User interaction can be unpredictable, and the user may choose to fully close the app or browser.
:::

### Step 5 - Get the status of the payment

To receive the result of the user action, you may poll the status of the payment via the
[`GET:/payments/{reference}`][get-payment-endpoint].

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
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
-H "Merchant-Serial-Number: YOUR-MSN" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-H "Vipps-System-Plugin-Name: acme-webshop" \
-H "Vipps-System-Plugin-Version: 4.5.6" \
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
payment or chosen to click `cancel` on the landing page or in the app,
the `state` property will be marked `ABORTED`. If the user did not act within
the payment expiration time, the `state` property will be marked `EXPIRED`.

For more details of the lifecycle of the payment session the
[`GET:/payments/{reference}/events`][get-payment-event-log-endpoint] endpoint.

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
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
-H "Merchant-Serial-Number: YOUR-MSN" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-H "Vipps-System-Plugin-Name: acme-webshop" \
-H "Vipps-System-Plugin-Version: 4.5.6" \
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
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
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
-H "Merchant-Serial-Number: YOUR-MSN" \
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
[Common topics: Capture](https://developer.vippsmobilepay.com/docs/common-topics/reserve-and-capture#capture)
for more details about the types of captures.

### (Optional) Step 7 - Refund the payment

To refund the captured amount, either partially or fully:
[`POST:/payments/{reference}/refund`][refund-payment-endpoint].

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
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
-H "Merchant-Serial-Number: YOUR-MSN" \
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
[Common topics: refund](https://developer.vippsmobilepay.com/docs/common-topics/refund)
for more details about refunds.

### (Optional) Step 8 - Cancel the payment

To cancel the payment, either fully or after a partial capture:
[`POST:/payments/{reference}/cancel`][cancel-payment-endpoint].

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
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
-H "Merchant-Serial-Number: YOUR-MSN" \
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
[Common topics: Cancel](https://developer.vippsmobilepay.com/docs/common-topics/cancel)
for more details about cancel.

## Next Steps

Now that you have completed your first payment,
read further to see the full range of possibilities within the ePayment API.

* [Capture the payment](operations/capture.md)
* [Payment modification, how to use cancel, capture and refund?](operations/README.md)
* [Using ePayment API in a shopper present context](features/customer-present-payments.md)
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
