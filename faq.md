---
sidebar_label: FAQ
sidebar_position: 130
description: Frequently asked questions for the ePayment API.
pagination_next: null
pagination_prev: null
---

# Frequently asked questions

For common questions, see [Common API FAQ](https://developer.vippsmobilepay.com/docs/faqs).

## Is Express Checkout available in the ePayment API?

Not yet.

It's possible to implement the ePayment API for everything except Express Checkout,
and use the
[eCom API](https://developer.vippsmobilepay.com/docs/APIs/ecom-api/)
just for
[Express Checkout](https://developer.vippsmobilepay.com/docs/APIs/ecom-api/vipps-ecom-api/#express-checkout-payments).

With the API platform, you benefit from a shared API framework for all the APIs.
This means that all APIs use the same API keys, authentication methods, terminology, and error message formats.
Integrating with our APIs is straightforward, and combining functionalities from multiple APIs is easy.

## Can payments be "mixed and matched" between the eCom API and the ePayment API?

Yes, to a certain extent.
The eCom API uses the ePayment API *behind the scenes*.

The ePayment API is *backwards compatible* with the eCom API,
but the eCom API is *forwards compatible* with the ePayment API:

* If the payments are initiated with the
  [eCom API](https://developer.vippsmobilepay.com/docs/APIs/ecom-api/),
  it's possible to capture, cancel, query, and adjust using the ePayment API.
* Payments initiated with the ePayment API cannot be modified or retrieved using the eCom API.

**Important:** Freestanding card payments are only available in the ePayment API,
so payments initiated with the ePayment API may fail if the eCom API is used.

See:
[Migration from the eCom API to the ePayment API](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/migration/).

## What do all the errors mean?

Here is an overview of errors you may get from the ePayment API.
They should be self-explanatory, but please let us know if they can be improved.

Errors responses have this format:

```json
{
   "type":"http://example.com",
   "title":"string",
   "detail":"string",
   "traceId":"string"
}
```

We will change the above format to match the common
[Errors](https://developer.vippsmobilepay.com/docs/common-topics/errors/)
format by changing `traceId` to `instance`.
The ePayment documentation will be updated when that is done.

**Important:** The unique identifier of an error is the `type`.
When handling errors, you must always identify the type of error by the `type` field.
The `title` and `description` may be updated at any time, without warning,
to improve the API and make the error messages easier to understand.

| Title                   | Description                                                  | Comment                    |
| ----------------------- | ------------------------------------------------------------ | -------------------------- |
| Amount too small        | The amount is too small. Amounts are specified in minor units, like øre or cent. | For NOK the minimum is 100.|
| Amount invalid          | The amount is invalid. Amounts must be integers, no decimals. They are specified in minor units, like øre or cent. | A common error is to specify amounts with decimals, sometimes due to rounding errors. |
| Express payment not allowed | Express payment is not allowed for this sales unit. | |
| Missing static shipping details | Express payments with static shipping details require a list of shipping options. | |
| No cards   | The user does not have any payment cards. | The user must add a valid card in the app. |
| Payment limit exceeded | The merchant's payment request limit is exceeded. | |
| Operation not supported |  The attempted payment operation is not supported. | |
| Capture amount too high | The total capture amount exceeds the reserved amount. Cannot capture a higher amount than the amount the user has accepted. Check the payment details. | |
| Cannot capture before reservation | The amount you tried to capture is not reserved. The user must accept the payment before capture can be done. | |
| Cannot capture a cancelled payment | Cannot capture a payment that has been cancelled. Check the payment event log. | See [Cancellations](https://developer.vippsmobilepay.com/docs/common-topics/cancel/) and [`GET:/epayment/v1/payments/{reference}/events`](https://developer.vippsmobilepay.com/api/epayment/#tag/QueryPayments/operation/getPaymentEventLog).  |
| Capture period expired | Payments can only be captured up to 180 days after reservation. See the FAQ. | See [Reserve and capture](https://developer.vippsmobilepay.com/docs/common-topics/reserve-and-capture/). |
| Capture idempotency conflict | The capture request in an idempotent retry must be identical to the previous request(s). | See [Idempotency](https://developer.vippsmobilepay.com/docs/common-topics/http-headers/#idempotency). |
| Cannot cancel a captured payment | Cannot cancel a payment that has been captured. Check the payment event log. | See [Cancellations](https://developer.vippsmobilepay.com/docs/common-topics/cancel/) and [`GET:/epayment/v1/payments/{reference}/events`](https://developer.vippsmobilepay.com/api/epayment/#tag/QueryPayments/operation/getPaymentEventLog). |
| Cannot cancel a non-reserved payment | Cannot cancel a payment that is not reserved. Check the payment event log. | See [Cancellations](https://developer.vippsmobilepay.com/docs/common-topics/cancel/) and [`GET:/epayment/v1/payments/{reference}/events`](https://developer.vippsmobilepay.com/api/epayment/#tag/QueryPayments/operation/getPaymentEventLog).  |
| Cancel period expired | Payments can only be canceled within 180 days of the reservation. See the FAQ. | See [Cancellations](https://developer.vippsmobilepay.com/docs/common-topics/cancel/).  |
| Cannot cancel pending | Cannot cancel a pending payment. | See [Cancellations](https://developer.vippsmobilepay.com/docs/common-topics/cancel/). |
| Order processing | Too many concurrent requests. The payment is being processed. | |
| Internal error | Internal error. This may be caused by an incorrect API request. Please check the request. See the status page. | |
| Payment already refunded | Cannot refund a payment that has already been refunded. Check the payment event log. | See [`GET:/epayment/v1/payments/{reference}/events`](https://developer.vippsmobilepay.com/api/epayment/#tag/QueryPayments/operation/getPaymentEventLog). |
| Not enough refundable | Cannot refund more than the available amount. Check the payment event log. | See [`GET:/epayment/v1/payments/{reference}/events`](https://developer.vippsmobilepay.com/api/epayment/#tag/QueryPayments/operation/getPaymentEventLog). |
| Refund period expired | Payments can only be refunded within 365 days of the reservation. See the FAQ. | See [Reserve and capture](https://developer.vippsmobilepay.com/docs/common-topics/reserve-and-capture/). |
| Refund idempotency conflict | The request in an idempotent retry must be identical to the previous request(s). | See [Idempotency](https://developer.vippsmobilepay.com/docs/common-topics/http-headers/#idempotency). |
| Attempted refund before reservation | Cannot refund a payment that is not reserved. Check the payment event log. | See and [`GET:/epayment/v1/payments/{reference}/events`](https://developer.vippsmobilepay.com/api/epayment/#tag/QueryPayments/operation/getPaymentEventLog). |
| Invalid phone number | The phone number is invalid. Phone numbers must be in MSISDN format: Country code and subscriber number, but no prefix. | |
| `merchantinfo.StaticShippingDetailsPrefix.missing` | Dynamic express payments require the shipping detail prefix. | |
| Customer not found | The phone number does not belong to a Vipps or MobilePay user, or the user cannot pay businesses. We cannot give more details. | |
| Idempotency error | Reference `acme-shop-123-order123abc` already exists. | See [Idempotency](https://developer.vippsmobilepay.com/docs/common-topics/http-headers/#idempotency). |
| Reference not found | The reference `acme-shop-123-order123abc` does not exist for MSN 123456 | |
| Idempotency error | Idempotency-Key `49ca711a-acee-4d01-993b-9487112e1def` already exists. | See [Idempotency](https://developer.vippsmobilepay.com/docs/common-topics/http-headers/#idempotency). |
| Invalid URL | The parameter `htt://example.com` is invalid. | |
| Missing required parameter | The parameter `something-something` is required. | |
| Direct capture not allowed | The sales unit with MSN `123456` is not allowed to use direct capture. | |
| Reserve capture not allowed | The sales unit with MSN `123456` is not allowed to use reserve capture. | |
| Skip landing page not allowed | The sales unit with MSN `123456` is not allowed to skip the landing page. | |
| Long-living payment not allowed | The sales unit with MSN `123456` is not allowed to perform long living payments. | |
| Payment cannot be cancelled | Reference `acme-shop-123-order123abc` cannot be cancelled. Invalid state: `something-something`. | See [Cancellations](https://developer.vippsmobilepay.com/docs/common-topics/cancel/). |
| Payment cannot be refunded | Reference `acme-shop-123-order123abc` cannot be refunded. Invalid state: `something-something`. | |
| Payment cannot be captured | Reference `acme-shop-123-order123abc` cannot be captured. Invalid state: `something-something`. | |
| Payment cannot be created | Reference `acme-shop-123-order123abc` cannot be created. Invalid state: `something-something`. | |
| Payment is already reserved | The payment with reference `acme-shop-123-order123abc` has already been reserved. | |
| Invalid scope | The scope `something-something` is invalid. | |
| Illegal scope | The scope `something-something` is illegal. Are you asking for more than you are allowed to? | |
| Approve failed | Force approve payment failed (this is only available in the test environment). Reason: `something-something`. | |
| Expiration date invalid | The expiration date `something-something` is invalid. | |
| ExpiresAt not allowed | Cannot set `ExpiresAt` for flow `something-something`. | |
