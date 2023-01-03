## Capture

When a payment is initiated with `$.directCapture = false` you must [Capture]([capture-payment-endpoint](https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/capturePayment)) a payment in order to initiate settlement of the authorised funds.

Captured funds will be settled to the merchants settlement account after two business days. See [Settlement Information](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/settlements) for more details.

A capture can be made in full, or partially if desired. The capture amount must be defined in capture API request.
