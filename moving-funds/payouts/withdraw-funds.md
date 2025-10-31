---
description: >-
  Send money from your Flash Payments balances to Australian bank accounts or
  internationally per your approved use case.
---

# Withdraw funds

To make a withdrawal, you need to execute the `createWithdrawal` mutation as below.

{% hint style="info" %}
You must have enough [balance](../../accounts/master-balance/) in your account for the chosen `currency` to make a withdrawal.
{% endhint %}

{% tabs %}
{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript"><strong>const bodyJSON = {
</strong>  variables: {
    input: {
      amount: 1000,
      currency: "AUD",
      sender: {
        companyName: "Acme LLC",
        address: {
          street: "1 Jon St PORTLAND VA 54321",
          country: "US",
        },
      },
      recipient: {
        bsb: "370370",
        accountNo: "123123123",
        companyName: "Acme Pty Ltd",
        currency: "AUD",
        accountIdType: "BSB",
        address: {
          street: "1 Main St SYDNEY NSW 2000",
          country: "AU",
        },
      },
      externalReference: "invoice #1234",
      externalId: "12344321",
      idempotencyKey: "12344321",
    },
  },
  query: `
 mutation ($input: CreateWithdrawalInput!) { 
  createWithdrawal(input: $input) { 
    success code message withdrawal { 
      id status amount currency 
    } 
  } 
}`,
 };  
</code></pre>
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($input: CreateWithdrawalInput!) {
  createWithdrawal(input: $input) {
    success
    code
    message
    withdrawal {
      id
      status
      amount
      currency
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": {
    "amount": 1000,
    "currency": "AUD",
    "sender": {
      "companyName": "Acme LLC",
      "address": {
        "street": "1 Jon St PORTLAND VA 54321",
        "country": "US"
      }
    },
    "recipient": {
      "bsb": "370370",
      "accountNo": "123123123",
      "companyName": "Acme Pty Ltd",
      "currency": "AUD",
      "accountIdType": "BSB",
      "address": {
        "street": "1 Main St SYDNEY NSW 2000",
        "country": "AU"
      }
    },
    "externalReference": "invoice #1234",
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
    "createWithdrawal": {
      "success": true,
      "code": "SUCCESS",
      "message": "Withdrawal was created",
      "withdrawal": {
        "id": "6904332f42b934e1954a734e",
        "status": "INITIALISED",
        "amount": 1000,
        "currency": "AUD"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Payment reference - `externalReference`

Arbitrary text, which will be seen in the ultimate recipient's bank statement. E.g. `"invoice #123"`. Will be eventually truncated to `18` ASCII chars if delivered via Australia's old (DE, Direct Entry) payment system. However, if you choose to use the real-time NPP network, then the maximum length is `280` chars.

### Recipient - `recipient` object or  `recipientId`

You can either [pre-create recipients](../recipients/#create-a-recipient) and provide us with their ID or submit a valid recipient object directly to `createWithdrawal`  as shown in the above example. The recipient's Australian account must be either `BSB` or `PAYID` (coming soon).

### Sender - `sender` object, `senderId`, `subClientId` , or neither

In the above `createWithdrawal` example, you had to first [pre-create a sender](../senders.md#create-a-sender) and use `senderId` as an input. Alternatively, if your account is configured to disburse funds **on behalf of** your [sub-clients](https://developer.flash-payments.com/sub-clients), you may provide us with the sub-client ID, and the withdrawal created will be linked to that sub-client. In this case, the `subClientId` will be used as the sender and will be reported to the government.

To use `subClientId` as the sender for your withdrawal, please execute the `createWithdrawal` mutation as below.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      amount: 1000, 
      currency: "AUD",
      externalReference: "invoice #1234",
      recipientId: "5ba89a6b35a2b327b81ffc3b",
      subClientId: "3fbj71b1dc328d56g94g9375",
      externalId: "123443212",
      idempotencyKey: "123443212",
    },
  }, 
  query: ` 
mutation ($input: CreateWithdrawalInput!) {
  createWithdrawal(input: $input) {
    success code message    
    withdrawal {
      id status amount currency    
    }  
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
 mutation($input: CreateWithdrawalInput!) {
  createWithdrawal(input: $input) {
    success
    code
    message
    withdrawal {
      id
      status
      amount
      currency
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
 { 
    "input": {
      "amount": 1000,
      "currency": "AUD",
      "externalReference": "invoice #1234",
      "recipientId": "5ba89a6b35a2b327b81ffc3b",
      subClientId: "3fbj71b1dc328d56g94g9375",
      "externalId": "123443212",
      "idempotencyKey": "123443212"
    }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
 {
  "data": {
    "createWithdrawal": {
      "success": true,
      "code": "OK",
      "message": "Scheduled for immediate execution",
      "withdrawal": {
        "id": "60711af8c078ba061f623531",
        "status": "PENDING",
        "amount": 1000,
        "currency": AUD
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
We are legally obliged to collect the actual sender and beneficiary details. Please do not send us intermediate organisation details such as exchanges, banks, gateways, etc.

If it is an intermediate, please see [Instiutions](withdraw-funds.md#institutions) instead.

Please always send us the ultimate sender and recipient. If sending funds to yourself, please provide your own details. See the schema in [Playground](https://api.uat.flash-payments.com.au/) for other recipient details options.

If sending funds from yourself, there's an option to use your company's Flash account details as sender by default. Please consider the example below.
{% endhint %}

If your company is the ultimate sender for a withdrawal, you can skip both the `senderId` and `subClientId`. In this situation, we will use your company’s Flash account as the sender for the transaction. Please note that a new sender record will not be created in this case.

Please execute the following `createWithdrawal` mutation to use your company's Flash account details as sender.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      amount: 500, 
      currency: "AUD",
      externalReference: "invoice #123",
      recipientId: "661e293b14a0e678c37fa327",
      externalId: "1234567890",
      idempotencyKey: "0987654321",
    },
  }, 
  query: ` 
mutation ($input: CreateWithdrawalInput!) {
  createWithdrawal(input: $input) {
    success code message    
    withdrawal {
      id status amount currency
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
 mutation($input: CreateWithdrawalInput!) {
  createWithdrawal(input: $input) {
    success
    code
    message
    withdrawal {
      id
      status
      amount
      currency
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
      "amount": 500,
      "currency": "AUD",
      "externalReference": "invoice #123",
      "recipientId": "661e293b14a0e678c37fa327",
      "externalId": "1234567890",
      "idempotencyKey": "0987654321"
    }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "createWithdrawal": {
      "success": true,
      "code": "SUCCESS",
      "message": "Withdrawal was created",
      "withdrawal": {
        "id": "67cb69f2ee6c254315bb1c3d",
        "status": "INITIALISED",
        "amount": 500,
        "currency": "AUD",
        "sender": {
          "firstName": "John",
          "lastName": "Smith",
          "companyName": "Smith Consulting Pty Ltd"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Instructing Institutions

An organisation that instructed you to make a withdrawal. This data is mandatory if you submit this withdrawal on behalf of another financial institution.

{% hint style="info" %}
For more information please see [Institutions](../institutions.md).
{% endhint %}

#### Using existing institutions `instructingInstitutionId`

This optional field refers to an existing `Institution` that was created earlier in the Flash Connect interface or via this API.

#### Create institutions on the fly using the `instructingInstitution` field

Optional field that allows you to provide [Institution](../institutions.md) details without pre-creating one. Once passed, Flash Payments will create the Institution for you. Before creating an institution, we will try to find an existing one:

* By `instructingInstitution.externalId` if present.
* By `instructingInstitution.businessNumber` AND `instructingInstitution.address.country`

### Callback (aka [Webhook](../../basics/webhooks/adhoc-webhooks.md)) URI

We recommend against continuous polling for withdrawal status changes. Instead, please use `callbackUri`.

The optional `callbackUri` will be invoked several times during the processing of a withdrawal. These callbacks will usually occur soon (within several seconds) after the initial create withdrawal call - but may be delayed in some cases. The example JSON payloads can be found on the [Webhooks page](../../basics/webhooks/#example-payloads).

{% hint style="danger" %}
**Security note**

The callback (aka [webhook](../../basics/webhooks/adhoc-webhooks.md)) endpoint URI can be invoked by anyone on the internet. Thus opening up a potential attack vector. See [Webhooks](../../basics/webhooks/adhoc-webhooks.md) page to secure your data properly.
{% endhint %}
