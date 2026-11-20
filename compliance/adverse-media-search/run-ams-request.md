# Run AMS Request

## Adverse Media Search

To run a search, execute the `adverseMediaSearch` mutation as below.

#### Search for an individual

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      name: "John Smith",
      akaNames: ["Johnny Smith"],
      country: "AU",
      approximateDob: "1980-05",
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
    "name": "John Smith",
    "akaNames": ["Johnny Smith"],
    "country": "AU",
    "approximateDob": "1980-05"
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
        "status": "PENDING"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

#### Search for an organisation

Set `isCompany: true` when screening a business or other non-person entity.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      name: "Acme Pty Ltd",
      akaNames: ["Acme Trading"],
      isCompany: true,
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
    "name": "Acme Pty Ltd",
    "akaNames": ["Acme Trading"],
    "isCompany": true,
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
        "status": "PENDING"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

#### Name — `name`

Required. The full name of the individual or organisation as it is commonly written, 2 to 256 characters. Letters and numbers in any language, spaces and common punctuation (`.,'-/&@+`) are allowed.

#### Other names — `akaNames`

Optional. Up to 4 other names the subject is known by, such as aliases, trading or former names. They follow the same rules as `name` and are searched alongside it.

#### Individual or organisation — `isCompany`

Optional. `true` when `name` is an organisation. Defaults to `false` (an individual).

#### Country — `country`

Required. A two-letter ISO 3166-1 alpha-2 country code (e.g. `AU`, `US`, `GB`), used to focus the search on the subject's country.

#### Date of birth — `approximateDob`

Optional, for individuals. Send only the parts you know: `YYYY`, `YYYY-MM` or `YYYY-MM-DD`. The individual must be at least 18 and no more than 120 years old.

Articles stating an age or birth year that does not fit this date of birth are marked with `ageMismatch: true`, as they are most likely about a different person with the same name.

#### Background scan — tracking progress

The scan runs in the background and may take several minutes. The mutation returns immediately with the request `id`. Use it to poll the [`amsRequest`](query-ams-requests.md) query, or subscribe to webhook events to be notified when the scan progresses.

Webhook events fired during a scan:

| Event             | Status        |
| ----------------- | ------------- |
| `ams_initialised` | `INITIALISED` |
| `ams_pending`     | `PENDING`     |
| `ams_completed`   | `COMPLETED`   |
| `ams_failed`      | `FAILED`      |

See [AMS statuses](ams-statuses.md) for the full lifecycle and the result fields.

#### Deduplication

If you submit the same search more than once within the same UTC calendar day, the existing request is returned instead of creating a new one. Two searches are the same when `name`, `country`, `isCompany`, `approximateDob` and `akaNames` all match. A `FAILED` request is never reused. The `code` field tells you which case applied:

| Code                     | Meaning                                      |
| ------------------------ | -------------------------------------------- |
| `SCAN_SCHEDULED`         | New scan created and queued                  |
| `SCAN_ALREADY_SCHEDULED` | The same search is already in progress today |
| `SCAN_ALREADY_COMPLETED` | The same search already completed today      |

#### Monthly request limit

A free monthly limit applies to AMS requests. Once reached, `success: false` is returned with `code: LIMIT_EXCEEDED`. Contact support to increase your limit.
