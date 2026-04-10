---
description: >-
  Submit an adverse media search for an individual or organisation. The scan
  runs in the background — use the returned ID to track progress.
---

# Adverse Media Search

To run a search, execute the `adverseMediaSearch` mutation as below.

{% hint style="info" %}
This API must be enabled for your account. Please contact support if you receive `AMS_API_DISABLED`.
{% endhint %}

### Search for an individual

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      firstName: "John",
      lastName: "Smith",
      country: "AU",
    },
  },
  query: `
mutation ($input: AmsRequestInput!) {
  adverseMediaSearch(input: $input) {
    success
    code
    message
    amsRequest {
      id
      status
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($input: AmsRequestInput!) {
  adverseMediaSearch(input: $input) {
    success
    code
    message
    amsRequest {
      id
      status
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "input": {
    "firstName": "John",
    "lastName": "Smith",
    "country": "AU"
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "adverseMediaSearch": {
      "success": true,
      "code": "SCAN_SCHEDULED",
      "message": "Name scan scheduled",
      "amsRequest": {
        "id": "6820a4f3e1c2b5d8f0123456",
        "status": "INITIALISED"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Search for an organisation

Use `accountName` instead of individual name fields when screening a business or other non-person entity.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      accountName: "Acme Pty Ltd",
      country: "AU",
    },
  },
  query: `
mutation ($input: AmsRequestInput!) {
  adverseMediaSearch(input: $input) {
    success
    code
    message
    amsRequest {
      id
      status
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($input: AmsRequestInput!) {
  adverseMediaSearch(input: $input) {
    success
    code
    message
    amsRequest {
      id
      status
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "input": {
    "accountName": "Acme Pty Ltd",
    "country": "AU"
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "adverseMediaSearch": {
      "success": true,
      "code": "SCAN_SCHEDULED",
      "message": "Name scan scheduled",
      "amsRequest": {
        "id": "6820b1d9c3f4a7e2d0987654",
        "status": "INITIALISED"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Name — `firstName` / `lastName` / `middleName` or `accountName`

For individuals, provide at least one of `firstName` or `lastName`. The optional `middleName` improves matching accuracy.

For organisations, use `accountName` instead. You cannot combine `accountName` with individual name fields.

{% hint style="warning" %}
Either `accountName` **or** at least one of `firstName` / `lastName` is required. Providing both will result in a validation error.
{% endhint %}

### Country — `country`

A two-letter ISO 3166-1 alpha-2 country code (e.g. `AU`, `US`, `GB`). This is required and used to contextualise results — articles where the detected location does not match the supplied country are flagged via `locationMismatch`.

### Background scan — tracking progress

The scan runs in the background and may take several minutes. The mutation returns immediately with `status: INITIALISED`. Use the returned `id` to [poll for results](query-ams-requests.md#retrieving-a-single-ams-request) or subscribe to [webhook events](../../basics/webhooks/README.md) to be notified when the scan progresses.

Webhook events fired during a scan:

| Event | Status |
|---|---|
| `ams_initialised` | `INITIALISED` |
| `ams_pending` | `PENDING` |
| `ams_completed` | `COMPLETED` |
| `ams_failed` | `FAILED` |

See [AMS statuses](ams-statuses.md) for the full lifecycle.

### Deduplication

If you submit a search for the same name and country more than once within the same calendar day, the existing request is returned rather than creating a new one. The `code` field indicates whether the scan was newly scheduled or is already in progress or complete:

| Code | Meaning |
|---|---|
| `SCAN_SCHEDULED` | New scan created and queued |
| `SCAN_ALREADY_SCHEDULED` | A scan for this name and country is already in progress today |
| `SCAN_ALREADY_COMPLETED` | A completed scan for this name and country already exists today |

### Monthly request limit

A free monthly limit applies to AMS requests. Once reached, `success: false` is returned with `code: LIMIT_EXCEEDED`. Contact support to increase your limit.
