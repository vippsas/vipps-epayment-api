<!-- START_METADATA
---
title: Long-living payments
hide_table_of_contents: true
sidebar_position: 20
---
END_METADATA -->

# Long-living payments

The ePayment API supports long-living payments for paymentMethod type as `WALLET`, where the merchant can specify the expiration time when initiating the payment with [`POST:/epayment/v1/payments`](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments).

**Please note:** Sales units (i.e., Merchant Serial Numbers (MSNs)) must be especially approved to use this feature.
Vipps wants the user experience, including the standard timeout, to be as
consistent as possible, so `expiresAt` should only be used in special cases.
Please contact your key account manager (KAM) to get access to this feature.

Extending the payment expiration time is done by setting the `expiresAt` parameter in the
[`POST:/epayment/v1/payments`](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments)
request.
The `expiresAt` must be between 10 minutes and 28 days (40320 minutes) in the future.

Here is an example of a valid request body:

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
      "type":"wallet"
   },
   "reference":"acme-shop-123-order123abc",
   "returnUrl":"https://example.com/redirect?orderId=1512202",
   "userFlow":"PUSH_MESSAGE",
   "expiresAt":"2023-02-15T00:00:00Z"
}
```

This will send a push message to the customer's Vipps app on their mobile phone.

If the above is attempted for a sales unit that is not authorized, it will result in
an error similar to this:

```json
{
    "type": "",
    "title": "Long living Ecom not allowed",
    "status": 400,
    "detail": "Merchant serial number 123456 is not allowed to perform long living Ecom transactions",
    "instance": "/v1/payments/",
    "traceId": "00-631d10d7ee147e46941a0725ed2dcd6a-54bda84425e20140-01"
}
```

If the customer's phone number is unknown, the system can request with `userFlow` set to `QR`.
This will return the QR code for a payment with the expiration time that you have specified.

The customer either clicks on the notification or scans the QR code to complete the payment flow in the Vipps app.
