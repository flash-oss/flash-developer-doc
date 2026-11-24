---
description: Screen individuals and organisations against adverse media sources
---

# Adverse Media Search

Adverse Media Search (AMS) lets you screen an individual or organisation against web-based adverse media sources — news articles, court records, and other publicly available content.

{% hint style="info" %}
The feature is also available through the [Flash Connect](https://connect.flash-payments.com/) portal. You can run searches with the same inputs as the API (other names, organisation, date of birth), see each article's recommendation and details, re-run past searches, and monitor usage statistics without writing any code. This can be useful for getting familiar with the feature before integrating it into your application via the API.
{% endhint %}

You can [run a search](run-ams-request.md) via the `adverseMediaSearch` mutation or past searches via the `amsRequest` and `amsRequests` queries.\
\
Scans run in the background and can take several minutes. Use webhooks or poll the `amsRequest` query to track progress. See [AMS statuses](ams-statuses.md) for the full lifecycle.
