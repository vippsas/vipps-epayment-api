<!-- START_METADATA
---
sidebar_label: FAQ
sidebar_position: 130
description: Frequently asked questions for the ePayment API.
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Frequently asked questions

For common questions, see:

* [Common API FAQ](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/faqs)

## Direct Capture fails
Make sure you are configured for [Reserve Capture][reserve-capture-check], and make sure you add the flag `directCapture: true` to your [Create Payment][create-payment-endpoint] request.

[create-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/CreatePayments/operation/createPayment
[reserve-capture-check]: https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/faqs/reserve-and-capture-faq#how-can-i-check-if-i-have-reserve-capture-or-direct-capture
