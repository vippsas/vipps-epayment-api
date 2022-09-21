# How to setup Notification Webhooks

Notification webhooks can be setup in the Merchant Portal.

TODO

## Understanding Notification Webhooks


Notifications will be sent to the defined webhook URL after every adjustment to a payment. This includes:
- `CREATION`
- `AUTHORISATION`
- `CANCELLATION`
- `CAPTURE`
- `REFUND`
- `AUTHORISATION_ADJUSTMENT`
- `TERMINATION`

A notification will look something like this;

```json
{
  "reference": "UNIQUE-PAYMENT-REFERENCE",
  "pspReference": "497f6eca-6276-4993-bfeb-53cbbbba6f08",
  "paymentAction": "AUTHORISATION",
  "success": true,
  "amount": {
    "currency": "NOK",
    "type": "PURCHASE",
    "value": 1000
  },
  "processedAt": "2021-02-24T14:15:22Z",
  "idempotencyKey": "IDEMPOTENCY-KEY-OF-REQUEST"
}
```
Where the properties are:  

Parameter | Type | Required | Description
----------|------|----------|------------
`reference` | `string` | Y | Your unique reference to this payment given during `createPayment`
`pspReference` | `string` | Y | Vipps unique reference to this `paymentAction` for this payment
`paymentAction` | `string,enum` | Y | The action of this event, one of; `CREATION`, `AUTHORISATION`, `CANCELLATION`, `CAPTURE`, `REFUND`, `AUTHORISATION_ADJUSTMENT`, `TERMINATION`
`success` | `boolean` | Y | The outcome of the event
`amount` | `Object` | Y | The `currency` and `value` of the payment in minor units
`processedAt` | `string` | Y | The timestamp the `paymentAction` was completed at
`idempotencyKey` | `string` | Y | The `idempotencyKey` of the triggering payment api request. Note: The `idempotencyKey` provided during `createPayment` will then be used for `CREATION`, `AUTHORISATION` and `TERMINATION` as they are the result of a single api request.


Delivery of notifications should be handled idempotently, as Vipps will operate this service under a "at least once" delivery mechanism. Idempotent handling should be base on the `pspReference` which will be unique per event.

The "at least once" principle will operate with a back-off delivery mechanism where if the delivery of an event does not respond to Vipps with HTTP status `200 OK`, Vipps will schedule re-delivery again. Consecutive failures will result in larger time gaps between each attempt. Please see the table below for back-off periods.

| Attempt | Back off (wait time until next delivery) |
| --------|----------------------------------------- |
| 1-5     | 2 seconds                                |
| 6-10    | 15 seconds                               |
| 10-20   | 1 minute                                 |
| 20-50   | 15 minutes                               |
| 50+     | 1 hour                                   |

Events will be partitioned by `reference` and failed delivery of one event will stop further delivery of other events in that partition until the failed event can be successfully delivered. For example: if an `AUTHORISATION` event fails to be delivered, the subsequent `CAPTURE` event on that payment `reference` will not be delivered until failing event can be processed. This is to ensure consistency in the merchants systems.
