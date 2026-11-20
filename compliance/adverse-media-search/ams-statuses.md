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

The `results` field lists every article analysed during the scan, including ones that turned out not to be adverse. Each `AmsWebSearchResult` includes:

| Field                | Description                                                                                         |
| -------------------- | --------------------------------------------------------------------------------------------------- |
| `title`              | Title of the web page                                                                               |
| `link`               | URL of the article                                                                                  |
| `snippet`            | Web search snippet                                                                                  |
| `recommendation`     | Recommendation for this article: `accept`, `review` or `flag`                                       |
| `summary`            | One-sentence summary of the article                                                                 |
| `matchedFullName`    | Full name of the subject exactly as printed in the article, if found                                |
| `nameSimilarity`     | How closely the name in the article matches the searched name, 0–100. Compares names only           |
| `adversityScore`     | Overall adversity score, 0–100. Higher = more adverse content found                                 |
| `amlCtfScore`        | Money-related crime or wrongdoing score, 0–100 (fraud, tax evasion, sanctions breaches, laundering) |
| `role`               | Role of the subject in the article: `perpetrator`, `victim` or `other`                              |
| `roleDetails`        | Further detail on the role or alleged involvement                                                   |
| `extractedCountry`   | ISO country code extracted from the article, if detected                                            |
| `extractedState`     | State or region extracted from the article, if detected                                             |
| `dob`                | Date of birth extracted from the article, if detected                                               |
| `ageInArticle`       | Age of the person extracted from the article, if detected                                           |
| `earliestAssumedBirthYear` | Earliest year the person could have been born in, judging by the article, if it gives such a clue |
| `latestAssumedBirthYear` | Latest year the person could have been born in, judging by the article, if it gives such a clue |
| `ageMismatch`        | `true` if the age or birth year in the article does not fit the supplied `approximateDob`           |
| `articlePublishDate` | Publication date of the article, if detected                                                        |
| `paywall`            | `true` if the article is behind a paywall and could not be fully analysed                           |
| `lookup`             | Keywords found in the article and their occurrence counts                                           |
