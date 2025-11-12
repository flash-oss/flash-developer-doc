---
description: How to send orchestrated account-to-account payments which include FX
---

# Send funds

To make a payment from AUD to a different currency, you need to execute the `createPayment` mutation as below.

{% hint style="info" %}
Note that you must have enough AUD balance to make an outbound AUD payment.
{% endhint %}

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      fromCurrency: "AUD",
      toCurrency: "GBP",
      size: 1000,
      currency: "AUD",
      reason: "BUSINESS",
      sourceOfFunds: "BUSINESS_FUNDS",
      externalReference: "my ref 221b",
      sender: {
        companyName: "Acme AU Ltd",
        address: {
          street: "1 Hay St SYDNEY NSW 2000",
          country: "AU",
        },
      },
      recipient: {
        iban: "GB33BARC20001234567890",
        companyName: "Acme GB Ltd",
        currency: "GBP",
        accountIdType: "IBAN",
        address: {
          street: "1 Main St LONDON SW1A 1AA",
          country: "GB",
        },
      },
      externalId: "12344321",
      idempotencyKey: "12344321",
    },
  },
  query: ` 
mutation ($input: PaymentInput!) {
  createPayment(input: $input) {
    success code message
    payment {
      id status size
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
 mutation($input: PaymentInput!) {
  createPayment(input: $input) {
    success
    code
    message
    payment {
      id
      status
      size
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": { 
    "fromCurrency": "AUD",
    "toCurrency": "GBP",
    "size": 1000,
    "currency": "AUD",
    "reason": "BUSINESS",
    "sourceOfFunds": "BUSINESS_FUNDS",
    "externalReference": "my ref 221b",
    "externalId": "12344321",
    "idempotencyKey": "12344321"
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "createPayment": {
      "success": true,
      "code": "SUCCESS",
      "message": "Scheduled for immediate execution",
      "payment": {
        "id": "60711af8c078ba061f623531",
        "status": "OPEN",
        "size": 1000
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Recipient - `recipient` object or `recipientId`

You can either [pre-create recipients](https://developer.flash-payments.com/moving-funds/recipients#create-a-recipient) and provide us with the `recipientId` or submit a valid `recipient` object directly to `createPayment` as shown in the above example. We recommend the latter where possible, as you won’t need to send an extra HTTP request. Please note that a new recipient record won’t be created in this case.

{% hint style="warning" %}
We are legally obliged to collect the actual sender and beneficiary details. Please do not send us intermediate organisation details such as exchanges, banks, gateways, etc.

If it is an intermediate, please see [Instiutions](send-funds.md#institutions) instead.

Please always send us the ultimate sender and recipient. If sending funds to yourself, please provide your own details. See the schema in [Playground](https://api.uat.flash-payments.com.au/) for other recipient details options.

If sending funds from yourself, there's an option to use your company's Flash account details as sender by default. Please consider the examples below.
{% endhint %}

### Sender - `sender` object, `senderId`, `subClientId` , or neither <a href="#sender-senderid-or-subclientid-or-neither" id="sender-senderid-or-subclientid-or-neither"></a>

Just like submitting recipient information, you can either [pre-create a sender](../../moving-funds/senders.md#create-a-sender) and provide us with the `senderId` or directly submit a valid `sender` object to `createPayment` as shown in the above example. Please note that a new sender record won’t be created in the latter case.\
\
Alternatively, if your account is configured to send funds **on behalf of** your [sub-clients](https://developer.flash-payments.com/sub-clients), you may provide us with the `subClientId` and the FX payment created will be linked to that sub-client. In this case the sub-client will be used as the sender and reported to the government.

To use `subClientId` as the sender for your withdrawal, please execute the `createPayment` mutation as below.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      fromCurrency: "AUD",
      toCurrency: "GBP",
      size: 1000,
      currency: "AUD",
      reason: "BUSINESS",
      sourceOfFunds: "BUSINESS_FUNDS",
      externalReference: "my ref 221b",
      subClientId: "6092360bf40f2dgc52f85cf1",
      recipient: {
        iban: "GB33BARC20001234567890",
        companyName: "Acme GB Ltd",
        currency: "GBP",
        accountIdType: "IBAN",
        address: {
          street: "1 Main St LONDON SW1A 1AA",
          country: "GB",
        },
      },
      externalId: "123443212",
      idempotencyKey: "123443212",
    },
  },
  query: ` 
mutation ($input: PaymentInput!) {
  createPayment(input: $input) {
    success code message
    payment {
      id status size
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
 mutation($input: PaymentInput!) {
  createPayment(input: $input) {
    success
    code
    message
    payment {
      id
      status
      size
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": { 
    "fromCurrency": "AUD",
    "toCurrency": "GBP",
    "size": 1000,
    "currency": "AUD",
    "reason": "BUSINESS",
    "sourceOfFunds": "BUSINESS_FUNDS",
    "externalReference": "my ref 221b",
    "subClientId": "6092360bf40f2dgc52f85cf1",
    "externalId": "12344321",
    "idempotencyKey": "12344321"
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "createPayment": {
      "success": true,
      "code": "SUCCESS",
      "message": "Scheduled for immediate execution",
      "payment": {
        "id": "60711bg8d078cb061g623531",
        "status": "OPEN",
        "size": 1000
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

If your company is the ultimate sender for an FX payment, you can skip both the `senderId` and `subClientId`. In this situation, we will use your company’s Flash account as the sender for the payment. Please note that a new sender record will not be created in this case.

Please execute the following `createPayment` mutation to use your company's Flash account details as sender.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      fromCurrency: "AUD",
      toCurrency: "GBP",
      size: 1000,
      currency: "AUD",
      reason: "BUSINESS",
      sourceOfFunds: "BUSINESS_FUNDS",
      externalReference: "my ref 2234",
      recipient: {
        iban: "GB33BARC20001234567890",
        companyName: "Acme GB Ltd",
        currency: "GBP",
        accountIdType: "IBAN",
        address: {
          street: "1 Main St LONDON SW1A 1AA",
          country: "GB",
        },
      },
      externalId: "0123443210",
      idempotencyKey: "0000012344321000",
    },
  },
  query: ` 
mutation ($input: PaymentInput!) {
  createPayment(input: $input) {
    success code message
    payment {
      id status size
      sender {
        firstName lastName companyName
      }
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
 mutation($input: PaymentInput!) {
  createPayment(input: $input) {
    success
    code
    message
    payment {
      id
      status
      size   
      sender {
        firstName 
        lastName 
        companyName
      }
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": { 
    "fromCurrency": "AUD",
    "toCurrency": "GBP",
    "size": 1000,
    "currency": "AUD",
    "reason": "BUSINESS",
    "sourceOfFunds": "BUSINESS_FUNDS",
    "externalReference": "my ref 2234",
    "externalId": "0123443210",
    "idempotencyKey": "0000012344321000"
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "createPayment": {
      "success": true,
      "code": "SUCCESS",
      "message": "Scheduled for immediate execution",
      "payment": {
        "id": "67d8e98caaa23286e1a1fd00",
        "status": "OPEN",
        "size": 1000,
        "sender": {
          "firstName": "John",
          "lastName": "Smith",
          "companyName": "Smith Consulting Pty Ltd"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Callback (aka [Webhook](../../basics/webhooks/adhoc-webhooks.md)) URI

We recommend against continuous polling for payment status changes. Instead, please use `callbackUri`.

The optional `callbackUri` will be invoked several times during the processing of a payment. These callbacks will usually occur soon (within several seconds) after the initial create payment call - but may be delayed in some cases. The example JSON payloads can be found on the [Webhooks page](../../basics/webhooks/#example-payloads).

Please note that `toAmount` (or `fromAmount`) and other fluctuating payment properties can change during payment execution.

{% hint style="danger" %}
**Security note**

The callback (aka [webhook](../../basics/webhooks/adhoc-webhooks.md)) endpoint URI can be invoked by anyone in the internet. Thus opening up a potential attack vector. See [Webhooks](../../basics/webhooks/adhoc-webhooks.md) page to secure your data properly.
{% endhint %}
