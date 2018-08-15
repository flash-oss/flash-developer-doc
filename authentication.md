---
description: Get your access token
---

# Authentication

{% hint style="warning" %}
This mutation is subject to change during BETA period.

With current implementation the token will **expire in 4 hours**.
{% endhint %}

Here is an example of the login query.

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  login(input: { email: "you@example.com", password: "12345678" }) {
    token
    message
    code
    success
  }
}
```
{% endtab %}

{% tab title="Response" %}
```graphql
{
  "data": {
    "login": {
      "token": "YOUR_TOKEN",
      "message": "OK",
      "code": "SUCCESS",
      "success": true
    }
  }
}
```
{% endtab %}
{% endtabs %}

If using [GraphQL Playground](https://api.flash-fx.com/) then click the "HTTP HEADERS" on the bottom left and paste there the following \(replace the token with the value you have just received form the above mutation\):

```javascript
{
  "authorization": "Bearer YOUR_TOKEN"
}
```

