<!-- START_METADATA
---
title: Checklist
sidebar_position: 10
---
END_METADATA -->

# Vipps ePayment API Checklist

<!-- START_COMMENT -->

ℹ️ Please use the web documentation:
[Vipps Technical Documentation](https://vippsas.github.io/vipps-developer-docs/).

<!-- END_COMMENT -->

API version: 1.0.

## Checklist

- [ ] Integrate _all_ the [API endpoints][epayment-api-reference-url]:
  - [ ] Full and partial Capture [`POST:epayment/v1/{reference}/capture`][capture-payment-endpoint]
  - [ ] Cancel [`POST:epayment/v1/{reference}cancel`][cancel-payment-endpoint]
  - [ ] Full and partial Refund [`POST:epayment/v1/{reference}refund`][refund-payment-endpoint]
  - [ ] Details [`GET:epayment/v1/{reference}`][get-payment-endpoint]
  - For examples of requests and responses, see the [Postman collection](tools/vipps-epayment-api-postman-collection.json) and [environment](https://github.com/vippsas/vipps-developers/blob/master/tools/vipps-api-global-postman-environment.json).
- [ ] Send the [Vipps HTTP headers](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/common-topics/http-headers)
      in all API requests for better tracking and troubleshooting
      (mandatory for partners and platforms, who must send these headers as part of the checklist approval):
  - [ ] `Merchant-Serial-Number`
  - [ ] `Vipps-System-Name`
  - [ ] `Vipps-System-Version`
  - [ ] `Vipps-System-Plugin-Name`
  - [ ] `Vipps-System-Plugin-Version`
- [ ] Follow the [orderId recommendations](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/common-topics/orderid).
- [ ] We recommend using Vipps Order management, as this is a massive benefit for the end user experience. It is mandatory for merchants using
      ["Vipps Assisted Content Monitoring"](https://vippsas.github.io/vipps-developer-docs/docs/APIs/order-management-api/vipps-order-management-api#vipps-assisted-content-monitoring).


- [ ] Correctly handle callbacks from Vipps, both for successful and unsuccessful payments.
      See the API documentation for
      [how callback URLs are built](vipps-ecom-api.md#callback-endpoints),
      make test calls to make sure you handle the `POST` requests correctly.
      Vipps does not have capacity to manually do this for you.


  - [ ] Callback [`POST:[callbackPrefix]/v2/payments/{orderId}`](https://vippsas.github.io/vipps-developer-docs/api/ecom#tag/Merchant-Endpoints/operation/transactionUpdateCallbackForRegularPaymentUsingPOST)
- [ ] Make sure to log and handle all errors.
       All integrations should to display errors in a way that the user can see and understand them.
- [ ] Avoid Integration pitfalls
  - [ ] The Merchant _must not_ rely on `fallback` or `callback` alone, and must poll Payments Details [`GET:/epayment/v1/{reference}`][get-payment-endpoint].
        as documented (this is part of the first item in this checklist, but it's still a common error). For pure payment status polling, the ePayment API is recommended.
  - [ ] The merchant must handle that the `fallback` URL is opened in the default browser on the phone,
          and not in a specific browser, in a specific tab, in an embedded browser, requiring a session token, etc.
          See the API guide:
          [Recommendations regarding handling redirects](vipps-ecom-api.md#recommendations-regarding-handling-redirects).
          See the FAQ: [How can I open the fallback URL in a specific (embedded) browser?](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/faqs/common-problems-faq#how-can-i-open-the-fallback-url-in-a-specific-embedded-browser)
  - [ ] The Vipps branding must be according to the
          [Vipps design guidelines](https://github.com/vippsas/vipps-design-guidelines).
  - [ ] Make sure your customer service, etc has all the tools and information they need
          available in _your_ system, through the APIs listed in the first item in this checklist,
          and that they do not need to visit
          [portal.vipps.no][portal-url]
          for normal work.

## Flow to go live for direct integrations

1. The merchant orders
   [Vipps på Nett](https://www.vipps.no/produkter-og-tjenester/bedrift/ta-betalt-paa-nett/ta-betalt-paa-nett/).
2. Vipps completes customer control (KYC, PEP, AML, etc).
3. The merchant receives an email from Vipps saying that they can log in with
   BankID on
   [portal.vipps.no][portal-url]
   and retrieve API keys.
4. The merchant completes all checklist items above.
   Please double check to avoid mistakes.
5. The merchant verifies the integration in the test environment by checking that
   there are test IDs (`orderId`) in the
   [Vipps test environment](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/test-environment),
   with the following states:
    - A complete order ending in `REFUND`
      ([`/refund`][refund-payment-endpoint] request).
    - A complete order ending in `VOID`
      ([`/cancel`][cancel-payment-endpoint] request).
    - In the test environment this must be verified using the API itself.
6. The Merchant verifies the integration in the production environment (similar to step 5):
    - A complete order ending in `REFUND`
      ([`/refund`][refund-payment-endpoint]).
    - For _reserve capture_: A complete order ending in `VOID`
      ([`/cancel`][cancel-payment-endpoint] request after reserve).
    - We recommend checking this using both the API itself and the API Dashboard available under "Utvikler" on
      [portal.vipps.no][portal-url].
    - **Please note:** Vipps does not do any kind of activation or make any changes based on this checklist.
      The API keys for the production environment are made available on
      [portal.vipps.no][portal-url]
      as soon as the customer control (see step 2) is completed, independently of this checklist.
7. The Merchant goes live 🎉

## Flow to go live for direct integrations for partners

See: [Vipps partners](https://vippsas.github.io/vipps-developer-docs/docs/vipps-partner/).


[portal-url]: https://portal.vipps.no
[epayment-api-reference-url]: https://vippsas.github.io/vipps-developer-docs/api/epayment
[create-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/CreatePayments/operation/createPayment
[get-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/QueryPayments/operation/getPayment
[get-payment-event-log-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/QueryPayments/operation/getPaymentEventLog
[cancel-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/cancelPayment
[capture-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/capturePayment
[refund-payment-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/refundPayment
[adjust-authorization-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/adjustAuthorization
[force-approve-endpoint]: https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/ForceApprove/operation/forceApprove