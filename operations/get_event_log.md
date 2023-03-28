<!-- START_METADATA
---
sidebar_label: Get payment event log
sidebar_position: 100
---
END_METADATA -->

# Get payment event log

A [`GET:/payments/{reference}/events`][get-payment-endpoint]
request will return a list of all events for a given payment.

An example response would look like this:
```json
[
   {
      "reference":"UNIQUE-PAYMENT-REFERENCE",
      "pspReference":"83b3fdb0-9547-4ec5-bdb7-5600ce70e792",
      "name":"CREATED",
      "amount":{
         "currency":"NOK",
         "value":1000
      },
      "timestamp":"2023-03-27T10:51:44.5333258Z",
      "idempotencyKey":"19986263-dedf-4b8e-a424-8d042eee4013",
      "success":true
   },
   {
      "reference":"UNIQUE-PAYMENT-REFERENCE",
      "pspReference":"1304787731",
      "name":"AUTHORIZED",
      "amount":{
         "currency":"NOK",
         "value":1000
      },
      "timestamp":"2023-03-27T10:51:59.378Z",
      "idempotencyKey":"19986263-dedf-4b8e-a424-8d042eee4013",
      "success":true
   },
   {
      "reference":"UNIQUE-PAYMENT-REFERENCE",
      "pspReference":"2018200034",
      "name":"CAPTURED",
      "amount":{
         "currency":"NOK",
         "value":250
      },
      "timestamp":"2023-03-27T10:52:07.748Z",
      "idempotencyKey":"19986263-dedf-4b8e-a424-8d042eee4013",
      "success":true
   },
   {
      "reference":"UNIQUE-PAYMENT-REFERENCE",
      "pspReference":"2019200034",
      "name":"CANCELLED",
      "amount":{
         "currency":"NOK",
         "value":750
      },
      "timestamp":"2023-03-27T10:52:15.616Z",
      "success":true
   }
]
```


[get-payment-event-log-endpoint]: https://developer.vippsmobilepay.com/api/epayment#tag/QueryPayments/operation/getPaymentEventLog
