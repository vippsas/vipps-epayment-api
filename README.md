---
title: Introduction to the ePayment API
sidebar_label: Introduction
sidebar_position: 1
hide_table_of_contents: true
description: Use the ePayment API to create various online and in-store payment flows.
pagination_next: null
pagination_prev: null
---

# ePayment API

![Vipps](./images/vipps.png) *Available for Vipps unless otherwise stated.*

![MobilePay](./images/mp.png) *Available for MobilePay in selected markets at the [Vipps MobilePay joint platform launch](https://www.vippsmobilepay.com/about).*

The ePayment API enables your customers to pay with Vipps or MobilePay from online shops and at the Point Of Sale (POS).

![ePayment online process](images/ePayment_online.png)

## History

The ePayment API is the new API that replaces both the
[eCom API](https://developer.vippsmobilepay.com/docs/APIs/ecom-api) and the
[MobilePay App Payments API](https://developer.mobilepay.dk/docs/app-payments).
It is based on everything we have learned through those APIs over several years.

This API contains new functionality, so you should migrate it as soon as possible
to take advantage of the improved user experience.

* [Migrate from the eCom API](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/migration/)
* [Migrate from the MobilePay APIs](https://developer.vippsmobilepay.com/docs/mp-migration-guide/)

## User flows

The ePayment API supports several user flows and can be used for any type of payment situation:

* Payments online (remote sales)
* Payments in physical situations (when the customer is present)
* Payments initiated on the customer's phone, or on a different device (including the merchant's device)
* Payments using QR codes

See
[Create payment](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/operations/create/)
for details.

## Features

The features of the ePayment API include:

* [Profile sharing](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/features/profile-sharing)
* [Long-Living payments](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/features/long-living-payments)
* [Freestanding card payments](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/features/free-standing-card-payments)
* [QR payments](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/features/qr-payments)
* [Webhooks](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/features/webhooks)

The ePayment API enables merchants to accept payments using both the app and cards without the app:

* [App](how-it-works/vipps-epayment-api-how-it-works-online.md#1-pay-with-vipps)
* [Credit and debit cards](features/free-standing-card-payments.md)

## How it works

* [ePayment API: How It Works online](./how-it-works/vipps-epayment-api-how-it-works-online.md):
  Enable your customers to pay with Vipps or MobilePay online or in your app.
* [ePayment API: How it works in the store](./how-it-works/vipps-epayment-api-how-it-works-in-store.md):
  How the ePayment API can be integrated in your Point Of Sale (POS) system.

## Next steps

* [API quick start](quick-start.md): Run the basic examples in curl or Postman.
* [API features](features/README.md): Learn about the ePayment API features.
* [API checklist](checklist.md): Complete the checklist for direct and POS integrations.
* [API spec](https://developer.vippsmobilepay.com/api/epayment): Go straight to the endpoint specifications.

If you're new to the platform, see
[Getting started](https://developer.vippsmobilepay.com/docs/getting-started/)
for information about API keys, product activation, and the test environment.
