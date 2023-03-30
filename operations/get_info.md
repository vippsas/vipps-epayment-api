<!-- START_METADATA
---
sidebar_label: Get payment
sidebar_position: 90
description: Get payment with the ePayment API.
---
END_METADATA -->

# Get payment

A [`GET:/payments/{reference}`][get-payment-endpoint]
request will return a point in time snapshot of a given payment.

An example response would look like this:
```json
{
    "aggregate": {
        "authorizedAmount": {
            "currency": "NOK",
            "value": 1000
        },
        "cancelledAmount": {
            "currency": "NOK",
            "value": 1000
        },
        "capturedAmount": {
            "currency": "NOK",
            "value": 0
        },
        "refundedAmount": {
            "currency": "NOK",
            "value": 0
        }
    },
    "amount": {
        "currency": "NOK",
        "value": 1000
    },
    "state": "AUTHORIZED",
    "paymentMethod": {
        "type": "WALLET"
    },
    "profile": {},
    "pspReference": "37c34d8c-2649-448e-864b-060d5d93e4c4",
    "reference": "UNIQUE-PAYMENT-REFERENCE"
}
```

In the case where the merchant requested `scopes` in the
[Create payment][create-payment-endpoint]
request, the `profile` object will contain a `sub` identifying the customer,
after the customer has authorized the payment.

For example (truncated):

```json
{
  "state": "AUTHORIZED",
  "profile" : {
    "sub": "c06c4afe-d9e1-4c5d-939a-177d752a0944"
  }
}
```

The `sub` can then be used to retrieve the user's info with the
[Userinfo API](https://developer.vippsmobilepay.com/docs/APIs/userinfo-api):
[`GET:/userinfo/{sub}`](https://developer.vippsmobilepay.com/api/userinfo#operation/getUserinfo).

[get-payment-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/QueryPayments/operation/getPayment
[create-payment-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments/operation/createPayment
