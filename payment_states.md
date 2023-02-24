<!-- START_METADATA
---
title: Payment states
id: payment-states
pagination_prev: Null
pagination_next: Null
sidebar_label: Payment session states
sidebar_position: 15
---
END_METADATA -->

# Payment session states


Once a payment is `CREATED`, several modification actions can be made. Modification actions are defined as separate endpoints in the api. These are:

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
