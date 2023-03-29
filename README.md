<!-- START_METADATA
---
title: Introduction to the ePayment API
sidebar_label: Introduction
sidebar_position: 1
hide_table_of_contents: true
description: Use the ePayment API to create various online payment flows.
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# ePayment API

<!-- START_COMMENT -->

ℹ️ Please use the website:
[Vipps MobilePay Technical Documentation](https://developer.vippsmobilepay.com/docs/APIs/epayment-api).

<!-- END_COMMENT -->

The new ePayment API is designed from scratch, based on everything we have
learned through the
[eCom API](https://developer.vippsmobilepay.com/docs/APIs/ecom-api)
over several years.

The main benefits are:

* Support for different flows, like pay by QR code, out-of-the-box without the "hacks" required in the eCom API
* Support for long-lived payments (payment requests from merchants) that are valid up to 28 days
* Support for free-standing card payments: Pay with VISA and MasterCard without the Vipps app
* Uses the
  [Webhooks API](https://developer.vippsmobilepay.com/docs/APIs/webhooks-api)
  to always reliably send information back to the merchant or partner

The ePayment API allows merchants to accept payments using the following payment methods:

* [Vipps](how-it-works/vipps-epayment-api-how-it-works-online.md#1-pay-with-vipps) (the app)
* [Credit and debit cards](features/free-standing-card-payments.md) without Vipps.
  **Please note:** Card payment is not available in test environment.

## How it works

* [ePayment API: How It Works](./how-it-works/vipps-epayment-api-how-it-works-online.md):
  Let your customers pay with Vipps MobilePay online or in your app.
* [ePayment API: How it works in the store](./how-it-works/vipps-epayment-api-how-it-works-in-store.md):
  How the ePayment API can be integrated in your Point Of Sale (POS) system.

## Next steps

See
[Getting Started](https://developer.vippsmobilepay.com/docs/vipps-developers/vipps-getting-started)
for information about API keys, product activation, how to make API calls, etc.

Review the detailed documentation found here:

* [API quick start](quick-start.md): Quick start guide.
* [API features](features/README.md): Developer guide for Vipps ePayment API.
* [API checklist](checklist.md): For direct and POS integrations.
* [API spec](https://developer.vippsmobilepay.com/api/epayment): ePayment API reference specifications.
