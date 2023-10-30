<!-- START_METADATA
---
title: ePayment API changelog
sidebar_label: Changelog
sidebar_position: 200
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Changelog

All notable changes to the current API will be documented in this file.
To learn about API versioning, see
[API Lifecycle](https://developer.vippsmobilepay.com/docs/knowledge-base/api-lifecycle/).

## September 2023

* v1.4.0: Added `DKK` and `EUR` currencies. Only allowed for Danish and Finnish merchants, respectively.

## August 2023

* v1.3.0: Added `metadata` to the `CreatePaymentRequest`. The `metadata` is returned in the `/epayment/v1/payments/{reference}`

## July 2023

* v1.2.3: Added the possibility of sending `receipt` data through the ePayment API. Receipts are generated in the
  [Order Management API](https://developer.vippsmobilepay.com/docs/APIs/order-management-api/).

## June 2023

* v1.2.2: Added `personalQr` to the `Customer` properties in `/epayment/v1/payments`.

## May 2023

* v1.2.1: Removed the deprecated `paymentAction` properties.

## April 2023

* v1.1.4: Added the [ForceApprove](https://developer.vippsmobilepay.com/api/epayment#tag/ForceApprove) endpoint.

## March 2023

* v1.1.3: Removed Direct Capture from ePayment API.
* v1.1.2: Removed `receipt` from `CreatePaymentRequest`.

## December 2022

* ePayment API v1.1.1
