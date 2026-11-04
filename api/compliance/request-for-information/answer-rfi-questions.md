---
description: Submit answers with plaintext or documents
---

# Answer RFI questions

Use the `answerRfiQuestion` mutation to submit an answer for a single question on a `PENDING` RFI. Files are sent inline as base64 in the same call — there is no separate upload step or multipart request.

### **What kind of answer to send**&#x20;

The shape of the answer depends on the question's `fileUploadOption`:

| fileUploadOption | What to send                                                                                                                            |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `PREFERRED`      | A document is expected — send `files`. If the document is not available, send `text` with the reason instead. Sending both is rejected. |
| `NONE`           | Send `text` only. Files are not accepted.                                                                                               |

### **File rules**

* Accepted extensions: `.pdf`, `.jpg`, `.jpeg`, `.png`, `.xls`, `.xlsx`. The extension determines the MIME type, so it must match the actual file.
* Up to **10 files** per question.
* Up to **10 MB** per file (raw bytes, before base64 encoding).
* `base64` must be the plain base64 body, with no `data:...;base64,` prefix.

### &#x20;**Answer with a file**

For a `PREFERRED` question.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      rfiId: "61f3a2c8d1e9b7a4c5d6e7f8",
      questionCode: "SENDER_ID_PROOF",
      files: [{ name: "passport.pdf", base64: "JVBERi0xLjQK..." }],
    },
  },
  query: `
mutation ($input: AnswerRfiQuestionInput!) {
  answerRfiQuestion(input: $input) {
    success
    code
    message
    rfi {
      id
      status
      questions { questionCode answered }
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($input: AnswerRfiQuestionInput!) {
  answerRfiQuestion(input: $input) {
    success
    code
    message
    rfi {
      id
      status
      questions { questionCode answered }
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": {
    "rfiId": "61f3a2c8d1e9b7a4c5d6e7f8",
    "questionCode": "SENDER_ID_PROOF",
    "files": [
      { "name": "passport.pdf", "base64": "JVBERi0xLjQK..." }
    ]
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "answerRfiQuestion": {
      "success": true,
      "code": "RECORDED",
      "message": "RFI reply recorded",
      "rfi": {
        "id": "61f3a2c8d1e9b7a4c5d6e7f8",
        "status": "PENDING",
        "questions": [
          { "questionCode": "SENDER_ID_PROOF", "answered": true },
          { "questionCode": "OTHER_TX_PURPOSE", "answered": false }
        ]
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### **Answer with plain text only**

For a `NONE` question, or for a `PREFERRED` question where the document is not available and you provide the reason instead.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      rfiId: "61f3a2c8d1e9b7a4c5d6e7f8",
      questionCode: "OTHER_TX_PURPOSE",
      text: "Payment for invoice #INV-2031, software development services.",
    },
  },
  query: `
mutation ($input: AnswerRfiQuestionInput!) {
  answerRfiQuestion(input: $input) {
    success
    code
    message
    rfi {
      id
      status
      questions { questionCode answered }
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($input: AnswerRfiQuestionInput!) {
  answerRfiQuestion(input: $input) {
    success
    code
    message
    rfi {
      id
      status
      questions { questionCode answered }
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": {
    "rfiId": "61f3a2c8d1e9b7a4c5d6e7f8",
    "questionCode": "OTHER_TX_PURPOSE",
    "text": "Payment for invoice #INV-2031, software development services."
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "answerRfiQuestion": {
      "success": true,
      "code": "RECORDED",
      "message": "RFI reply recorded",
      "rfi": {
        "id": "61f3a2c8d1e9b7a4c5d6e7f8",
        "status": "ASSESSING",
        "questions": [
          { "questionCode": "SENDER_ID_PROOF", "answered": true },
          { "questionCode": "OTHER_TX_PURPOSE", "answered": true }
        ]
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### **Completing the RFI**

Each question can be answered once — pick the questions with `answered: false`. When the last open question is answered, the RFI moves to `ASSESSING` in the same call (as in the response above) and an [`rfi_assessing`](../../basics/webhooks/#rfi_assessing) webhook is dispatched. No further action is required from you.

{% hint style="info" %}
Invalid submissions such as an unknown `questionCode`, an already-answered question, files on a `NONE` question, or a file-rule violation will be rejected as GraphQL errors. [See RFI response codes](rfi-response-codes.md).&#x20;
{% endhint %}
