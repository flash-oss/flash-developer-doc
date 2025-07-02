# Query withdrawals

#### Retrieving all your withdrawals

{% tabs %}
{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript">const bodyJSON = {
<strong>  variables: {
</strong>    input: {
    },
  },
  query: `
query ($input: WithdrawalQueryInput!) {
  withdrawals(input: $input) {   
    id
    recipient {
     firstName lastName
    }
  }
}`,
};  
</code></pre>
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: WithdrawalQueryInput!) {
  withdrawals(input: $input) {
    id
    recipient {
      firstName
      lastName
    } 
    # there are many other properties
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{ 
  "input": {
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "withdrawals": [
      {
        "id": "5b04c62ec0bf606bf216ae21",
        "recipient": {
          "firstName": "John"
          "lastName": "Smith"
        }
      },
      {
        "id": "5b04c6bfc0bf606bf216af06",
        "recipient": {
          "firstName": "John"
          "lastName": "Smith"
        }
      },
      {
        "id": "5b04c8e3c0bf606bf216b026",
        "recipient": {
          "firstName": "John"
          "lastName": "Smith"
        }
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

#### Retrieving some of your withdrawals

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      statuses: "CONFIRMED",
      maxCreatedAt: "2020-01-29",
    },
  },
  query: `
query ($input: WithdrawalQueryInput!) {
  withdrawals(input: $input) {   
    id createdAt 
  }
}`,
};  
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: WithdrawalQueryInput!) {
  withdrawals(input: $input) {
    id
    createdAt 
    # there are many other properties
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```graphql
{    
  # there are more query parameters available, see the API schema
  "input": {
    "statuses": "CONFIRMED",
    "maxCreatedAt": "2020-01-29"
  }  
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "withdrawals": [
      {
        "id": "5b04c62ec0bf606bf216ae21",
        "createdAt": "2020-01-17T07:21:20.247Z"
      },
      {
        "id": "5b04c6bfc0bf606bf216af06",
        "createdAt": "2018-08-13T05:45:28.698Z"
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

#### Retrieving a single withdrawal

{% tabs %}
{% tab title="JavaScript" %}
```graphql
const bodyJSON = {
  variables:{
    input: "5b04c62ec0bf606bf216ae21",
  },
  query: `
query ($input: ID) {  
  withdrawal(id: $input) {
    status createdAt amount
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: ID) {
  withdrawal(id: $input) {
    status
    createdAt
    amount
    # there are many other properties
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```graphql
{
  # there are more query parameters available, see the API schema
  "input": "5b04c62ec0bf606bf216ae21"
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "withdrawal": {
      "status": "CONFIRMED",
      "createdAt": "2020-01-17T07:21:20.247Z",
      "amount": 1000
    }
  }
}
```
{% endtab %}
{% endtabs %}
