---
description: Adverse media search request statuses
---

# AMS statuses

1. You submit a search. The request is created and queued.\
   `INITIALISED`
2. The background scan starts — web sources are fetched and analysed.\
   `INITIALISED` → `PENDING`
3. `PENDING` →
   * `COMPLETED` — the scan finished successfully. Results are available in the `results` field. _**Final status.**_
   * `FAILED` — the scan could not be completed due to an error. The request is not billable. _**Final status.**_

#### Most common status transitions

**Happy path**

`INITIALISED` → `PENDING` → `COMPLETED`

**Unhappy path**

`INITIALISED` → `PENDING` → `FAILED`

{% hint style="info" %}
A `FAILED` request is not billed. You may submit the same search again — a new request will be created and processed.
{% endhint %}

#### Recommendation

Each article in `results` carries a `recommendation`. Once a request reaches `COMPLETED`, its own `recommendation` is the most severe one among its articles:

| Value    | Meaning                                                                                    |
| -------- | ------------------------------------------------------------------------------------------ |
| `accept` | No adverse media found about the subject                                                   |
| `review` | Possibly adverse content about someone with a close name, or a paywalled article — check it |
| `flag`   | Highly adverse content naming the subject exactly                                          |

#### Results

The `results` field lists every article analysed during the scan, including ones that turned out not to be adverse. Each `AmsWebSearchResult` describes one article. See the GraphQL API schema for the fields.
