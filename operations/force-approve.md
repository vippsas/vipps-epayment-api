---
title: Force Approve the payment with the ePayment API
sidebar_label: Force Approve
id: force-approve
sidebar_position: 200
---

# Force Approve a payment


To facilitate automated testing in The Vipps Test Environment (MT) ePayments API provides a "force approve" endpoint
to avoid manual payment confirmation in the Vipps app:
[`POST:/payments/{reference}/approve`][force-approve-endpoint].



:::note
Important: All test users must manually approve at least one payment in Vipps (using the app)
before "force approve" can be used for that user. If this has not been done, you will get an error.
This is because the user needs to be registered as "bankID verified" in the backend, 
and this is happens automatically in the test environment when using Vipps (the app), but not with "force approve".
:::


[force-approve-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/ForceApprove
