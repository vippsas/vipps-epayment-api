---
sidebar_label: Payment operations
id: payment-operations
sidebar_position: 30
---

# Payment operations

The ePayment API exposes seven payment operations:

* [Create a payment](create.md)
* [Capture a payment](capture.md)
* [Cancel a payment](cancel.md)
* [Refund a payment](refund.md)
* [Get a payment](get_info.md)
* [Get payment event log](get_event_log.md)
* [Force approve](force-approve.md) (only available in the test environment)

See: [Payment states and modifications](../payment_states.md).

## Rate limiting

We have added rate limiting to our APIs (HTTP 429 Too Many Requests) to prevent fraudulent and wrongful behavior, and to increase the stability and security of our APIs. The limits should not affect normal behavior, but please contact us if you notice any unexpected behavior.

The "Key" column specifies what we consider to be the unique identifier, and
what we "use to count". The limits are of course not *total* limits.

| API                          | Limit          | Key                          | Explanation |
| ---------------------------- | -------------- | ---------------------------- | ----------- |
| [Create](create.md)          | 5 per minute   | reference + MSN              | Five calls per minute per unique reference |
| [Capture](capture.md)        | 5 per minute   | reference + MSN              | Five calls per minute per unique reference |
| [Cancel](cancel.md)          | 5 per minute   | reference + MSN              | Five calls per minute per unique reference |
| [Refund](refund.md)          | 5 per minute   | reference + MSN              | Five calls per minute per unique reference |
| [Get a payment](get_info.md) | 120 per minute | reference + subscription key | 120 calls per minute per unique reference |
| [Get payment event log](get_event_log.md) | 120 per minute | reference + subscription key | 120 calls per minute per unique reference |


**Please note:** The "Key" column is important. The above means that we allow two
[Create](create.md)  
calls per minute *per unique reference* for that MSN. This
is to prevent too many initiate calls for the same payment. The overall limit
for number of *different* payments is *far* higher than 2. The same goes for
[Capture a payment](capture.md):
You can make five capture calls per minute for
one unique `reference`, and the limit for capture calls for different `reference` values
is *far* higher.