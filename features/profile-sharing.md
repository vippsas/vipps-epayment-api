---
title: Profile sharing
sidebar_label: Profile sharing
hide_table_of_contents: true
sidebar_position: 10
---

# Profile sharing

The ePayment API lets merchants ask for the user's profile information (such as phone number or
email addrss) as part of the payment flow. We call this the `userinfo` flow.

To enable the possibility to fetch profile information for a user, the merchant
can add a `scope` parameter to
[`POST:/epayment/v1/payments`](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments/operation/createPayment).

See the available values for
[scope](https://developer.vippsmobilepay.com/docs/APIs/userinfo-api/#scope)
in the 
[Userinfo API](https://developer.vippsmobilepay.com/docs/APIs/userinfo-api/)
documentation.

To test this out, see the step-by-step instructions in the
[Quick start](../quick-start.md).

See the
[Userinfo API guide](https://developer.vippsmobilepay.com/docs/APIs/userinfo-api)
for more details.
