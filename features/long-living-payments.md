---
title: Long-living payments
hide_table_of_contents: true
sidebar_position: 20
---

# Long-living payments

The ePayment API supports long-living payments where the merchant
can specify the expiration time when initiating the payment with
[`POST:/epayment/v1/payments`](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments).

This is done by specifying `expiresAt` in the payment initiation request.
The `expiresAt` must be between 10 minutes and 28 days (40320 minutes) in the future.

**Please note:** This functionality is only available when using the `WALLET` payment method,
since the app is required (it does not work with
[freestanding card payments](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/features/free-standing-card-payments/)).

**Please note:** Sales units (i.e., Merchant Serial Numbers) must be especially approved to use this feature.
Vipps wants the user experience, including the standard timeout, to be as
consistent as possible, so it should only be used in special cases.
To request this feature: Please contact your key account manager, your partner manager, or
[customer service)(https://vipps.no/kontakt-oss/).

## Request

```json
{
   "amount":{
      "currency":"NOK",
      "value":49900
   },
   "customer":{
      "phoneNumber":4791234567
   },
   "paymentMethod":{
      "type":"WALLET"
   },
   "reference":"acme-shop-123-order123abc",
   "returnUrl":"https://example.com/redirect?reference=acme-shop-123-order123abc",
   "userFlow":"PUSH_MESSAGE",
   "expiresAt":"2023-02-15T00:00:00Z"
}
```

This will send a push message to the customer's app (specifed with `phoneNumber`),
and the customer confirms the payment.

If the customer's phone number is unknown, the the request can specify `userFlow` as `QR`.
This will return the QR code for a payment, including the expiration time specified.
The customer scans the QR code to complete the payment flow in the app.

If a payment is initiated with the `expiresAt` for a sales unit that is not allowed to use
the feature, the response will be an error.


## Response

The response is similar to a regular payment initiation.
