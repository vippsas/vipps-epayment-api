<!-- START_METADATA
---
title: Profile sharing
sidebar_label: Profile sharing
hide_table_of_contents: true
sidebar_position: 10
---
END_METADATA -->

# Profile sharing

Vipps offers the possibility for merchants to ask for the user's profile
information as part of the payment flow. We call this the `userinfo` flow.

To enable the possibility to fetch profile information for a user, the merchant
can add a `scope` parameter to the initiate call:
[`POST:/epayment/v1/payments`](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments/operation/createPayment).

To test this out, see the step-by-step instructions in the
[Quick start](../quick-start.md).

See the
[Userinfo API guide](https://developer.vippsmobilepay.com/docs/APIs/userinfo-api)
for more details.
