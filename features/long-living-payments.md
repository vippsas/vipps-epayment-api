<!-- START_METADATA
---
title: Long living payments
hide_table_of_contents: true
pagination_next: null
pagination_prev: APIs/epayment-api/getting-started
---
END_METADATA -->

The ePayment API supports long-living payments for paymentMethod type as `WALLET`, where the merchant can specify the expiration time when initiating the payment with POST:/epayment/v1/payments.

The expiration time is set with the expiresAt parameter.

See [Long living payments](https://vippsas.github.io/vipps-developer-docs/docs/vipps-solutions/long-expiry-time-for-payments-to-merchants) for more details.