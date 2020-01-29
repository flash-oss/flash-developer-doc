# Query withdrawals

#### Retrieving all your withdrawals

{% tabs %}
{% tab title="Query" %}
```graphql
{
  withdrawals {
    id
    recipient { fullName }
    # there are many other properties
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
          "fullName": "John Smith"
        }
      },
      {
        "id": "5b04c6bfc0bf606bf216af06",
        "recipient": {
          "fullName": "John Smith"
        }
      },
      {
        "id": "5b04c8e3c0bf606bf216b026",
        "recipient": {
          "fullName": "John Smith"
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
{% tab title="Query" %}
```graphql
{
  # there are more query parameters available, see the API schema
  withdrawals(input: { statuses: CONFIRMED, maxCreatedAt: "2020-01-29" }) {
    id
    createdAt
    # there are many other properties
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
{% tab title="Query" %}
```graphql
{
  # there are more query parameters available, see the API schema
  withdrawal(id: "5b04c62ec0bf606bf216ae21") {
    status
    createdAt
    amount
    # there are many other properties
  }
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



