---
title: eCom API checklist
sidebar_label: Checklist
sidebar_position: 100
description: Checklist for full integration with the ePayment API.
pagination_next: null
pagination_prev: null
---

# Checklist

## Endpoints to integrate

Integrate _all_ the [API endpoints](https://developer.vippsmobilepay.com/api/epayment). For examples of requests and responses, see the [Postman collection](/tools/vipps-epayment-api-postman-collection.json) and [environment](https://github.com/vippsas/vipps-developers/blob/master/tools/vipps-api-global-postman-environment.json).

| Endpoint | Comment |
|-----|-----------|
|     Create payment| [`POST:/epayment/v1/payments`](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments/operation/createPayment) |
|     Get Payment| [`POST:/epayment/v1/payments/{reference}`](https://developer.vippsmobilepay.com/api/epayment#tag/QueryPayments/operation/getPayment)|
|     Get Payment Event log| [`POST:/epayment/v1/payments/{reference}/events`](https://developer.vippsmobilepay.com/api/epayment#tag/QueryPayments/operation/getPaymentEventLog)|
|     Cancel payment| [`POST:/epayment/v1/payments/{reference}/cancel`](https://developer.vippsmobilepay.com/api/epayment#tag/AdjustPayments/operation/cancelPayment)|
|     Full and Partial capture payment| [`POST:/epayment/v1/payments/{reference}/capture`](https://developer.vippsmobilepay.com/api/epayment#tag/AdjustPayments/operation/capturePayment)|
|     Full and partial refund payment| [`POST:/epayment/v1/payments/{reference}/refund`](https://developer.vippsmobilepay.com/api/epayment#tag/AdjustPayments/operation/refundPayment)|

## Quality assurance

|  | Comment |
|-----|-----------|
|     Response handling| Make sure to handle all responses and states from the payment: `CREATED`, `ABORTED`, `EXPIRED`, `CANCELLED`, `CAPTURED`, `REFUNDED`, `AUTHORIZED` and `TERMINATED`.|
|     Error handling| Make sure to log and handle [all errors](https://developer.vippsmobilepay.com/docs/APIs/ecom-api/vipps-ecom-api.md#errors). All integrations should to display errors in a way that the users (customers and merchant employees/administrators) can see and understand them.|
|     HTTP Headers| Send the [Vipps HTTP headers](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/http-headers) in all API requests for better tracking and troubleshooting (mandatory for partners and platforms, who must send these headers as part of the checklist approval). |
|     Vipps Order management| We recommend using the [Vipps Order Management API](https://developer.vippsmobilepay.com/docs/APIs/order-management-api) to add receipts and/or images to the payment history. This is a great benefit for the end user experience. It is also mandatory for merchants using ["Vipps Assisted Content Monitoring"](https://developer.vippsmobilepay.com/docs/APIs/order-management-api/vipps-order-management-api#vipps-assisted-content-monitoring). |

## Avoid Integration pitfalls

|  | Comment |
|-----|-----------|
|     Reference recommendations| Follow our [reference recommendations](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/orderid). |
|     Polling recommendations| Follow our [polling recommendations](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/polling-guidelines) |
|     Handle redirects| The merchant must handle that the `fallback` URL is opened in the default browser on the phone, and not in a specific browser, in a specific tab, in an embedded browser, requiring a session token, etc. See the API guide: [Recommendations regarding handling redirects](https://developer.vippsmobilepay.com/docs/APIs/ecom-api/vipps-ecom-api.md#recommendations-regarding-handling-redirects). See the FAQ: [How can I open the fallback URL in a specific (embedded) browser?](https://developer.vippsmobilepay.com/docs/vipps-developers/faqs/common-problems-faq#how-can-i-open-the-fallback-url-in-a-specific-embedded-browser)|
|     Follow design guidelines| The Vipps branding must be according to the [Vipps design guidelines](https://developer.vippsmobilepay.com/docs/vipps-design-guidelines).|
|     Educate customer support| Make sure your customer service, etc. has all the tools and information they need available in _your_ system, through the APIs listed in the first item in this checklist, and that they do not need to visit [portal.vipps.no](https://portal.vipps.no) for normal work.|


## Flow to go live for direct integrations

1. The merchant orders
   [Vipps på Nett](https://www.vipps.no/produkter-og-tjenester/bedrift/ta-betalt-paa-nett/ta-betalt-paa-nett/).
2. Vipps completes customer control (KYC, PEP, AML, etc.).
3. The merchant receives an email from Vipps saying that they can log in with
   BankID on
   [portal.vipps.no](https://portal.vipps.no)
   and retrieve API keys.
4. The merchant completes all checklist items above.
   Please double-check to avoid mistakes.
5. The merchant verifies the integration in the test environment by checking that
   there are test IDs (`reference`) in the
   [Vipps test environment](https://developer.vippsmobilepay.com/docs/vipps-developers/test-environment),
   with the following states:
   - A complete order ending in `CAPTURED`
     ([`/capture`](https://developer.vippsmobilepay.com/api/epayment#tag/AdjustPayments/operation/capturePayment)
     request).
   - A complete order ending in `REFUNDED`
     ([`/refund`](https://developer.vippsmobilepay.com/api/epayment#tag/AdjustPayments/operation/refundPayment)
     request).
   - A complete order ending in `CANCELLED`
     ([`/cancel`](https://developer.vippsmobilepay.com/api/epayment#tag/AdjustPayments/operation/cancelPayment)
     request).
   - In the test environment this must be verified using the API itself.
6. The Merchant verifies the integration in the production environment (similar to step 5):
    - A complete order ending in `AUTHORIZED`, `CAPTURED`, `REFUNDED` and `CANCELLED`
      request.
    - We recommend checking this using both the API itself and the API Dashboard available under "Utvikler" on
      [portal.vipps.no](https://portal.vipps.no).  
    - **Please note:** Vipps does not do any kind of activation or make any changes based on this checklist.
      The API keys for the production environment are made available on
      [portal.vipps.no](https://portal.vipps.no)
      as soon as the customer control (see step 2) is completed, independently of this checklist.
7. The Merchant goes live 🎉

## Flow to go live for direct integrations for partners

See: [Vipps partners](https://developer.vippsmobilepay.com/docs/vipps-partner).

