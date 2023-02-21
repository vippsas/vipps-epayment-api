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
    [*] --> Created
Created : Payment created via api
Created --> Terminated
Terminated : The merchant has stopped the 
Terminated: process through the API
Terminated --> [*]
Created --> Aborted
Aborted : The end user has actively stopped the
Aborted: process by selecting Cancel or similar
Aborted --> [*]
Created --> Expired
Expired : The payment session has
Expired: expired due to inactivity
Expired --> [*]
Created --> Authorised
Authorised : User has approved payment
Authorised : Defined by ""$.aggregate.authorisedAmount"" object
Authorised --> Capture
Capture : Payment fully or partially captured
Capture : Defined by ""$.aggregate.capturedAmount"" object
Capture --> Refund
Refund : Payment fully or partially refunded
Refund : Defined by ""$.aggregate.refundedAmount"" object
Refund --> [*] : Once payment is \nfully refunded
Created --> Cancel : Not possible if the user is \nactively authorising the \npayment
Authorised --> Cancel 
Capture --> Cancel : Not possible after \nfull capture
Cancel : Cancels the remaining un-captured amount
Cancel : Defined by ""$.aggregate.cancelledAmount"" object
Cancel --> [*]
```