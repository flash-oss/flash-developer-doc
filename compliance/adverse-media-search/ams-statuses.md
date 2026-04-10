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

### Most common status transitions

#### Happy path

`INITIALISED` → `PENDING` → `COMPLETED`

#### Unhappy path

`INITIALISED` → `PENDING` → `FAILED`

{% hint style="info" %}
A `FAILED` request is not billed. You may submit the same search again — a new request will be created and processed.
{% endhint %}

### Results

Once a request reaches `COMPLETED`, the `results` field is populated with an array of `AmsWebSearchResult` objects. Each result represents a single web article found during the scan and includes:

| Field | Description |
|---|---|
| `title` | Title of the web page |
| `snippet` | Web search snippet |
| `link` | URL of the article |
| `adversityScore` | Overall adversity score, 0–100. Higher = more adverse content found |
| `personSimilarity` | Similarity between the search subject and the person in the article, 0–100 |
| `role` | Role of the subject in the article — e.g. `perpetrator`, `victim`, `other` |
| `roleDetails` | Further detail on the role or alleged involvement |
| `summary` | One-sentence summary of the article |
| `extractedCountry` | ISO country code extracted from the article, if detected |
| `extractedState` | State or region extracted from the article, if detected |
| `locationMismatch` | `true` if the article's detected location does not match the supplied country |
| `paywall` | `true` if the article is behind a paywall and could not be fully analysed |
| `exactFullNameMatch` | `true` if the article contains an exact match of the full name |
| `dob` | Date of birth extracted from the article, if detected |
| `ageInArticle` | Age of the person extracted from the article, if detected |
| `articlePublishDate` | Publication date of the article, if detected |
| `lookup` | Keywords found in the article and their occurrence counts |
