---
title: Payment states and modifications
sidebar_label: Payment states
sidebar_position: 15
description: Payment states and modifications
pagination_next: null
pagination_prev: null
---

# Payment states and modifications

A payment can have several _states_, and change states when a
_modification_ is made.

| State         | Description   |
| ------------- | ------------- |
| `CREATED`     | The payment has been initiated. |
| `AUTHORIZED`  | The user has accepted the payment. |
| `ABORTED`     | The payment has been actively stopped by the user. |
| `EXPIRED`     | The payment has expired. See [Timeouts](https://developer.vippsmobilepay.com/docs/knowledge-base/timeouts).|
| `TERMINATED`  | The merchant has stopped the payment. |

When a payment is initiated it has the state `CREATED`.
Once a payment is `AUTHORIZED`, several modification actions can be made:

* [Capture](./operations/capture.md)
* [Refund](./operations/refund.md)
* [Cancel](./operations/cancel.md)

Each modification action is defined as a separate endpoint in the API.

The following flow diagram describes when each modification action is applicable.

``` mermaid
stateDiagram
    [*] --> CREATED
CREATED : CREATED
CREATED : Payment created via API
CREATED --> TERMINATED
TERMINATED : TERMINATED
TERMINATED : The merchant has stopped the\nprocess through the API
TERMINATED --> [*]
CREATED --> ABORTED
ABORTED : ABORTED
ABORTED : The end user has actively stopped the\nprocess by selecting Cancel or similar
ABORTED --> [*]
CREATED --> EXPIRED
EXPIRED : EXPIRED
EXPIRED : The payment session has\nexpired due to inactivity
EXPIRED --> [*]
CREATED --> AUTHORIZED
AUTHORIZED : AUTHORIZED
AUTHORIZED : User has approved payment\nDefined by ""$.aggregate.authorizedAmount"" object
AUTHORIZED --> CAPTURED
CAPTURED : CAPTURED
CAPTURED : Payment fully or partially captured\nDefined by ""aggregate.capturedAmount"" object
CAPTURED --> REFUNDED
REFUNDED : REFUNDED
REFUNDED : Payment fully or partially refunded\nDefined by ""aggregate.refundedAmount"" object
REFUNDED --> [*] : Once payment is\nfully refunded
CREATED --> CANCELLED : Not possible if the user is \nactively authorizing the \npayment
AUTHORIZED --> CANCELLED
CAPTURED --> CANCELLED : Not possible after \nfull capture
CANCELLED : CANCELLED
CANCELLED : Cancel the remaining un-captured amount\nDefined by ""aggregate.cancelledAmount"" object
CANCELLED --> [*]
```
