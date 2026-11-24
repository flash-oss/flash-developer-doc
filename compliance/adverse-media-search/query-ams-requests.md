# Query AMS requests

#### Retrieving a single AMS request

Use the `amsRequest` query with the `id` returned from the `adverseMediaSearch` mutation to check the status of a scan or retrieve its results.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    id: "6820a4f3e1c2b5d8f0123456",
  },
  query: `
query ($id: ID!) {
  amsRequest(id: $id) {
    id
    name
    akaNames
    isCompany
    country
    approximateDob
    status
    recommendation
    createdAt
    results {
      title
      link
      recommendation
      adversityScore
      nameSimilarity
      summary
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($id: ID!) {
  amsRequest(id: $id) {
    id
    name
    akaNames
    isCompany
    country
    approximateDob
    status
    recommendation
    createdAt
    results {
      title
      link
      recommendation
      adversityScore
      nameSimilarity
      summary
      # there are many other result fields
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "id": "6820a4f3e1c2b5d8f0123456"
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "amsRequest": {
      "id": "6820a4f3e1c2b5d8f0123456",
      "name": "John Smith",
      "akaNames": ["Johnny Smith"],
      "isCompany": false,
      "country": "AU",
      "approximateDob": "1980-05",
      "status": "COMPLETED",
      "recommendation": "review",
      "createdAt": "2025-03-15T08:23:14.521Z",
      "results": [
        {
          "title": "Local businessman John Smith fined for tax evasion",
          "link": "https://example-news.com.au/articles/smith-tax",
          "recommendation": "review",
          "adversityScore": 72,
          "nameSimilarity": 100,
          "summary": "John Smith of Sydney was fined $45,000 for understating income over three financial years."
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
The `results` field is populated only once the scan reaches `COMPLETED` status. While the scan is `INITIALISED` or `PENDING`, `results` will be `null`.
{% endhint %}

#### Retrieving all your AMS requests

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {},
  },
  query: `
query ($input: AmsQueryInput) {
  amsRequests(input: $input) {
    id
    name
    country
    status
    createdAt
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: AmsQueryInput) {
  amsRequests(input: $input) {
    id
    name
    country
    status
    createdAt
    # there are many other properties
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "input": {}
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "amsRequests": [
      {
        "id": "6820a4f3e1c2b5d8f0123456",
        "name": "John Smith",
        "country": "AU",
        "status": "COMPLETED",
        "createdAt": "2025-03-15T08:23:14.521Z"
      },
      {
        "id": "6820b1d9c3f4a7e2d0987654",
        "name": "Acme Pty Ltd",
        "country": "AU",
        "status": "PENDING",
        "createdAt": "2025-03-15T09:01:44.008Z"
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

#### Filtering AMS requests

Use the `input` parameter to narrow results by status, date range, name, date of birth or country.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      statuses: ["COMPLETED"],
      minCreatedAt: "2025-03-01T00:00:00.000Z",
      maxCreatedAt: "2025-04-01T00:00:00.000Z",
      country: "AU",
    },
  },
  query: `
query ($input: AmsQueryInput) {
  amsRequests(input: $input) {
    id
    name
    country
    status
    createdAt
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: AmsQueryInput) {
  amsRequests(input: $input) {
    id
    name
    country
    status
    createdAt
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  # there are more filter parameters available, see the API schema
  "input": {
    "statuses": ["COMPLETED"],
    "minCreatedAt": "2025-03-01T00:00:00.000Z",
    "maxCreatedAt": "2025-04-01T00:00:00.000Z",
    "country": "AU"
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "amsRequests": [
      {
        "id": "6820a4f3e1c2b5d8f0123456",
        "name": "John Smith",
        "country": "AU",
        "status": "COMPLETED",
        "createdAt": "2025-03-15T08:23:14.521Z"
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

See `AmsQueryInput` in the GraphQL API schema for the filter fields.

{% hint style="info" %}
`amsRequests` returns at most 1,000 requests, newest first. To reach older requests, narrow the query with `minCreatedAt` / `maxCreatedAt`.
{% endhint %}
