<!-- START_METADATA
---
title: Profile sharing
hide_table_of_contents: true
pagination_next: null
pagination_prev: APIs/epayment-api/getting-started
---
END_METADATA -->

# Profile sharing

Vipps offers the possibility for merchants to ask for the user's profile information as part of the payment flow. We call this the `userinfo` flow.

To enable the possibility to fetch profile information for a user, the merchant can add a `$.profile.scope`
parameter to the initiate call:
[`POST:/epayment/v1/payments`](https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/CreatePayments/operation/createPayment).
See
[Userinfo API guide](https://vippsas.github.io/vipps-developer-docs/docs/APIs/userinfo-api).


## Userinfo call-by-call guide

Scenario: You want to complete a payment and get the name and phone number of
a customer. Details about each step are described in the sections below.

1. Retrieve the access token:
   [`POST:/accesstoken/get`](https://vippsas.github.io/vipps-developer-docs/api/access-token#tag/Authorization-Service/operation/fetchAuthorizationTokenUsingPost).
2. Add scope to the profile object and include the scope you wish to get
   access to (valid scope) before calling
   [`POST:/epayment/v1/payments`](https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/CreatePayments/operation/createPayment)
   Include the scopes you need access to (e.g., "name address email phoneNumber birthDate"), separated by spaces.
3. The user consents to the information sharing and perform the payment in Vipps.
4. Retrieve the `sub` by calling
   [`GET:/epayment/v1/payments/{reference}`](https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/QueryPayments/operation/getPayment)
5. Using the sub from step 4, call
   [`GET:/vipps-userinfo-api/userinfo/{sub}`](https://vippsas.github.io/vipps-developer-docs/api/userinfo#operation/getUserinfo)
   to retrieve the user's information.
   Do not include the ``Ocp-Apim-Subscription-Key`` header. See more information under [Userinfo call](#userinfo-call).

To test this out, see the step-by-step instructions in the
[Getting Started](../getting-started.md).

**Important note:** The API call to
[`GET:/vipps-userinfo-api/userinfo/{sub}`](https://vippsas.github.io/vipps-developer-docs/api/userinfo#operation/getUserinfo)
must *not* include the subscription key (the `Ocp-Apim-Subscription-Key` header) used for the eCom API.
This is because userinfo is part of Vipps Login and is therefore *not* under the same subscription,
and will result in a `HTTP Unauthorized 401` error.

### Get userinfo

Once the user completes the session, a unique identifier `sub` can be retrieved from the profile object in the response of the
[`GET:/epayment/v1/payments/{reference}`](https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/QueryPayments/operation/getPayment) endpoint.

Example `sub` format:

```json
"profile": {
    "sub": "c06c4afe-d9e1-4c5d-939a-177d752a0944"
}
```

This `sub` is a link between the merchant and the user and can be used to retrieve
the user's details from Vipps userinfo:
[`GET:/vipps-userinfo-api/userinfo/{sub}`](https://vippsas.github.io/vipps-developer-docs/api/userinfo#operation/getUserinfo)

The `sub` is based on the user's national identity number ("fødselsnummer"
in Norway), and does not change (except in very special cases).

**Please note:** It is recommended to get the user's information directly after
completing the transaction. There is a *time limit of 168 hours*
(one week) to retrieve the consented profile data from the `/userinfo` endpoint. This is to
better support merchants that depend on manual steps/checks in their process of
fetching the profile data. The merchant will get the information that is in the
user profile at the time when they actually fetch the information. This means
that the information might have changed from the time the user completed the
transaction and the fetching of the profile data.
