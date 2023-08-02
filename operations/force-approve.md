---
title: Force Approve the payment with the ePayment API
sidebar_label: Force Approve
id: force-approve
sidebar_position: 200
---

# Force Approve a payment

To simplify automated testing in the
[test environment](https://developer.vippsmobilepay.com/docs/test-environment/)
the ePayment API provides a "force approve" endpoint that confirms payments
without using the test app:
[`POST:/epayment/v1/payments/{reference}/approve`][force-approve-endpoint].

:::note
Important: All test users must manually approve at least one payment in Vipps MobilePay app
before
[`POST:/epayment/v1/payments/{reference}/approve`][force-approve-endpoint]
can be used for that user. If this has not been done, you will get an error.
This is because the user needs to be registered as "BankID verified" in the backend,
and this happens automatically in the test environment when using the Vipps MobilePay app,
but not with "force approve".
:::

[force-approve-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/ForceApprove
