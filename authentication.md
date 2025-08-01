---
description: Get your access token
---

# Authentication

Before doing any other API calls you have to obtain an auth token. It's a standard JWT token carrying the following payload:

```javascript
{
  ...

  "iat": 1620967717,
  "exp": 1621054117
}
```

{% hint style="info" %}
Tip: Use this handy website to parse the token contents: [jwt.io](https://jwt.io/)
{% endhint %}

The token lifetime is **4 hours** at this time. We might change this value in the future.

{% hint style="warning" %}
Warning! You can't log in more than once per second. This limit is in place to maintain platform stability.
{% endhint %}

To be more future-proof, it is recommended to parse the token payload and compare current time to the token's expiration time. JavaScript code:

```javascript
const seconds = JSON.parse(Buffer.from(token.split(".")[1], "base64url")).exp;
if (Date.now() >= seconds*1000) {
  // get new token
}
```

{% hint style="warning" %}
This `login` mutation is a subject to change in the future.
{% endhint %}

### Getting a token

1. After we enable you, go to the [API Playground](https://api.uat.flash-payments.com.au/), click **"DOCS"** on the right to explore the possibilities.
2. Find there the `login` mutation. Execute it to obtain your access token. For example:\
   `mutation { login(input: {email: "YOUR_EMAIL" password: "YOUR_PWD"}) {token message} }`
3. Click the **"HTTP HEADERS"** on the bottom and add this: `{"authorization": "Bearer YOUR_TOKEN"}`. Replace the `YOUR_TOKEN` with the token you just got.
4. Execute any other queries.

Here is an example of the login query.

{% hint style="warning" %}
We suggest always sending your queries and related data separately using the "QUERY VARIABLES" tab in the [API playground](https://api.uat.flash-payments.com.au/) or programmatically by [submitting the variables as JSON](https://developer.flash-payments.com/basics/sending-data-as-json).
{% endhint %}

{% tabs %}
{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript"><strong>const bodyJSON = {
</strong>  variables: {
    input: {
      email: "you@example.com",
      password: "12345678",
    },
  },
  query: `
mutation ($input: LoginInput!) {
  login(input: $input) {
    token message code success
  }
}`,
};
</code></pre>
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($input: LoginInput!) {
  login(input: $input) {
    token
    message
    code
    success
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": { 
    "email": "you@example.com", 
    "password": "12345678" 
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
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

If using [API Playground](https://api.uat.flash-payments.com.au/) then click the "HTTP HEADERS" on the bottom left and paste there the following (replace the `YOUR_TOKEN` with the value you have just received form the above mutation):

```javascript
{
  "authorization": "Bearer YOUR_TOKEN"
}
```

### Affiliation

Clients that have contractual agreements with more than one subsidiary of Flash Payments, can use the optional field `affiliation` to control which account to log into.

The `affiliation` field accepts one of two constant values - `FP_AUS` or `FP_LUX` -  which are passed unquoted.

```javascript
{
  "input": { 
    "email": "you@example.com", 
    "password": "12345678",
    "affiliation": FP_AUS 
  }
}
```

If the `affiliation` value does not correspond to an existent account, authentication will fail with code `UNREGISTERED` even if an account exists with another affiliation and the credentials are correct.

If the user has multiple affiliations and the field `affiliation` is omitted, authentication will fail with code `NEED_AFFILIATION`.
