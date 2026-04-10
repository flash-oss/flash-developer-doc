---
description: Screen individuals and organisations against adverse media sources
---

# Adverse Media Search

Adverse Media Search (AMS) lets you screen an individual or organisation against web-based adverse media sources — news articles, court records, and other publicly available content.

{% hint style="info" %}
This API must be enabled for your account before use. Please contact support.
{% endhint %}

You can [run a search](adverse-media-search.md) via the `adverseMediaSearch` mutation.

You can [retrieve past searches](query-ams-requests.md) via the `amsRequest` and `amsRequests` queries.

Scans run in the background and can take several minutes. Use [webhooks](../../basics/webhooks/README.md) or poll the `amsRequest` query to track progress. See [AMS statuses](ams-statuses.md) for the full lifecycle.
