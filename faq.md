---
sidebar_label: FAQ
sidebar_position: 130
description: Frequently asked questions for the ePayment API.
pagination_next: null
pagination_prev: null
---

# Frequently asked questions

For common questions, see: [Common API FAQ](https://developer.vippsmobilepay.com/docs/vipps-developers/faqs)

## Is Express Checkout available in the ePayment API?

Not yet.

It's possible to impement the ePayment API for everything except Express Checkout,
and use the
[eCom API](https://developer.vippsmobilepay.com/docs/APIs/ecom-api/)
just for
[Express Checkout](https://developer.vippsmobilepay.com/docs/APIs/ecom-api/vipps-ecom-api/#express-checkout-payments).

As mentioned on the [API platform overview page](https://developer.vippsmobilepay.com/docs/APIs):

"With the Vipps MobilePay API platform, you benefit from a shared API framework for all the APIs. This means that all APIs use the same API keys, authentication methods, terminology, and error message formats. Integrating with our APIs is straightforward, and combining functionalities from multiple APIs is easy."

## Can payments be "mixed and matched" between the eCom API and the ePayment API?

Yes, to a certain extent: If the payments were initiated with the
[eCom API](https://developer.vippsmobilepay.com/docs/APIs/ecom-api/)
it's possible to  captured, cancel, etc. using the ePayment API.

The eCom API uses the ePayment API “behind the scenes”.

**Important:** Freestanding card payments are only available in the ePayment API,
so payments initiated with the ePayment API may fail if the eCom API is used.
