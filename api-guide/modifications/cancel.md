## Cancel

If you no longer wish to initiate settlement of the remaining funds on a payment then you should [Cancel](https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/AdjustPayments/operation/cancelPayment) the payment. Cancelling a payment provides a good user experience and synchronizes the users bank statement and Vipps payment overview with their expectations from a merchant.

A payment can be cancelled via the api at any point until the payment is fully captured. A cancellation will release any remaining authorised funds on the customers bank account. This cancellation by the merchant via the api will result in `TERMINATED` state of the payment.

A cancel can also be performed before an authorisation if required. A cancel by the end user before `AUTHORISATION` will result in a `ABORTED` state of the payment.

> Note: It is not possible to cancel a payment while a user is actively authorizing the payment. Eg: The payment is under processing with the payment scheme, or the user is in a 3DSecure session.
