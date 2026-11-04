---
description: Retrieve your RFIs and their questions
---

# Query RFIs

### **Retrieving a single RFI**

Use the `rfi` query with the `id` from the `rfi_created` webhook to load the questions and the linked transactions. The query returns `null` if the RFI does not exist or does not belong to you.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    id: "61f3a2c8d1e9b7a4c5d6e7f8",
  },
  query: `
query ($id: ID!) {
  rfi(id: $id) {
    id
    status
    statusMessage
    deadline
    questions {
      questionCode
      questionText
      fileUploadOption
      answered
    }
    withdrawals { id status }
    deposits { id status }
    payments { id status }
    createdAt
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($id: ID!) {
  rfi(id: $id) {
    id
    status
    statusMessage
    deadline
    questions {
      questionCode
      questionText
      fileUploadOption
      answered
    }
    withdrawals { id status }
    deposits { id status }
    payments { id status }
    createdAt
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "id": "61f3a2c8d1e9b7a4c5d6e7f8"
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "rfi": {
      "id": "61f3a2c8d1e9b7a4c5d6e7f8",
      "status": "PENDING",
      "statusMessage": "We are waiting for your replies",
      "deadline": "2026-06-04T09:00:00.000Z",
      "questions": [
        {
          "questionCode": "SENDER_ID_PROOF",
          "questionText": "Sender's ID proof",
          "fileUploadOption": "PREFERRED",
          "answered": false
        },
        {
          "questionCode": "OTHER_TX_PURPOSE",
          "questionText": "Purpose of transfer",
          "fileUploadOption": "NONE",
          "answered": false
        }
      ],
      "withdrawals": [
        { "id": "61f3a2c8d1e9b7a4c5d6e7aa", "status": "REVIEWING" }
      ],
      "deposits": [],
      "payments": [],
      "createdAt": "2026-05-28T09:00:00.000Z"
    }
  }
}
```
{% endtab %}
{% endtabs %}

### **The key RFI fields**

| Field                                   | Description                                                                                                  |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| `status`                                | One of `PENDING`, `ASSESSING`, `CLOSED` — see [RFI statuses](rfi-statuses.md)                                |
| `statusMessage`                         | Human-readable explanation of the current status                                                             |
| `deadline`                              | Submission deadline. Respond before it passes                                                                |
| `questions`                             | The questions to answer. `answered` tells you which ones are still open                                      |
| `deposits` / `withdrawals` / `payments` | The transactions this RFI is gating, in the same shape as the `deposit`, `withdrawal`, and `payment` queries |

Each question's `fileUploadOption` tells you what kind of answer it expects — see [Answer RFI questions.](answer-rfi-questions.md)

### **Retrieving all RFIs**

Use the `rfis` query, for example to find everything still awaiting your response.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      statuses: ["PENDING"],
    },
  },
  query: `
query ($input: RfisInput) {
  rfis(input: $input) {
    id
    status
    deadline
    questions {
      questionCode
      answered
    }
    createdAt
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: RfisInput) {
  rfis(input: $input) {
    id
    status
    deadline
    questions {
      questionCode
      answered
    }
    createdAt
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": {
    "statuses": ["PENDING"]
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "rfis": [
      {
        "id": "61f3a2c8d1e9b7a4c5d6e7f8",
        "status": "PENDING",
        "deadline": "2026-06-04T09:00:00.000Z",
        "questions": [
          { "questionCode": "SENDER_ID_PROOF", "answered": false },
          { "questionCode": "OTHER_TX_PURPOSE", "answered": false }
        ],
        "createdAt": "2026-05-28T09:00:00.000Z"
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}



### **Filter fields on** `RfisInput`

| Field          | Description                                                                |
| -------------- | -------------------------------------------------------------------------- |
| `statuses`     | Filter by one or more [RFI statuses](rfi-statuses.md)                      |
| `minCreatedAt` | Return only RFIs created at or after this timestamp (ISO 8601, inclusive)  |
| `maxCreatedAt` | Return only RFIs created at or before this timestamp (ISO 8601, inclusive) |

All filters are optional and combined with an "AND" logic when more than one is supplied. With no `input`, all your RFIs will be returned.
