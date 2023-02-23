---
description: Two types of the webhooks
---

# Webhooks

The main triggers for all webhooks - are payment or withdrawal status changes. E.g. when a withdrawal goes from `PENDING` to `CONFIRMED` status.

There are two types of webhooks in FlashFX.

* [Regular webhooks](webhooks.md) - a URL would need to be saved to your [FlashConnect](https://connect.flash-fx.com/) settings. All types of events.
  * You can lookup the history of all the HTTP requests and responses, their JSON bodies and headers.
  * If there is no response we will show you what exactly the problem is: DNS issue, networking issue, 5XX response, etc.
  * You can receive webhooks when a deposit lands to your virtual bank account
* [Ad hoc webhooks](adhoc-webhooks.md) - you would need to provide a callback URL per payment/withdrawal while creating them. Only "payment" and "withdrawal" events are supported.

The webhooks HTTP POST calls will follow all the [standard HTTP redirects](https://developer.mozilla.org/en-US/docs/Web/HTTP/Redirections) (3XX codes).

### Security

#### Cryptographic signature

All webhook HTTP requests carry a cryptographic signature. Ad hoc and regular webhooks do it slightly different though.

#### FlashFX webhook request IP address

All webhook HTTP requests would be coming from these IP addresses:

* Development environment: 52.62.195.119
* Production environment: 52.62.138.234 and 52.62.3.195

### Example payloads

**Deposits**

<details>

<summary>deposit_initiated</summary>

<pre class="language-json"><code class="lang-json"><strong>{
</strong>  "event": "deposit_initiated",
  "id": "603f0198770d6595e3c83e0d",
  "amount": 100,
  "totalFee": 1,
  "currency": "AUD",
  "externalReference": "2233445566",
  "clearedAt": "2021-03-03T03:25:12.792Z",
  "statusMessage": "Deposit initiated",
  "recipient": {
    "accountName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387"
  },
  "sender": {
    "accountName": "ACME Inc",
    "companyName": "ACME Inc",
    "bankName": "Bank ACME",
  },
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}
</code></pre>

</details>

<details>

<summary>deposit_reviewing</summary>

<pre class="language-json"><code class="lang-json"><strong>{
</strong>  "event": "deposit_reviewing",
  "id": "603f0198770d6595e3c83e0d",
  "amount": 100,
  "totalFee": 1,
  "currency": "AUD",
  "externalReference": "2233445566",
  "clearedAt": "2021-03-03T03:25:12.792Z",
  "statusMessage": "Awaiting manual compliance",
  "recipient": {
    "accountName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387"
  },
  "sender": {
    "accountName": "ACME Inc",
    "companyName": "ACME Inc",
    "bankName": "Bank ACME",
  },
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}
</code></pre>

</details>

<details>

<summary>deposit_cleared</summary>

<pre class="language-json"><code class="lang-json"><strong>{
</strong>  "event": "deposit_cleared",
  "id": "603f0198770d6595e3c83e0d",
  "amount": 100,
  "totalFee": 1,
  "currency": "AUD",
  "externalReference": "2233445566",
  "clearedAt": "2021-03-03T03:25:12.792Z",
  "statusMessage": "Deposit cleared",
  "recipient": {
    "accountName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387"
  },
  "sender": {
    "accountName": "ACME Inc",
    "companyName": "ACME Inc",
    "bankName": "Bank ACME",
  },
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}
</code></pre>

</details>

<details>

<summary>deposit_cancelled</summary>

```json
{
  "event": "deposit_cancelled",
  "id": "603f0198770d6595e3c83e0d",
  "amount": 100,
  "totalFee": 1,
  "currency": "AUD",
  "externalReference": "2233445566",
  "clearedAt": "2021-03-03T03:25:12.792Z",
  "statusMessage": "Cancelled by: john@example.com : ",
  "recipient": {
    "accountName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387"
  },
    "sender": {
    "accountName": "ACME Inc",
    "companyName": "ACME Inc",
    "bankName": "Bank ACME",
  },
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}
```

</details>

<details>

<summary>deposit_refunding</summary>

```json
{
  "event": "deposit_refunding",
  "id": "603f0198770d6595e3c83e0d",
  "amount": 100,
  "totalFee": 1,
  "refundAmount": 99,
  "currency": "AUD",
  "externalReference": "2233445566",
  "refundReason": "Client refund request",
  "statusMessage": "Deposit refunded",
  "refundedAt": "2021-03-03T03:28:43.936Z",
  "clearedAt": "2021-03-03T03:25:12.792Z",
  "recipient": {
    "accountName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387"
  },
    "sender": {
    "accountName": "ACME Inc",
    "companyName": "ACME Inc",
    "bankName": "Bank ACME",
  },
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}
```

</details>

<details>

<summary>deposit_refunded</summary>

```json
{
  "event": "deposit_refunded",
  "id": "603f0198770d6595e3c83e0d",
  "amount": 100,
  "totalFee": 1,
  "refundAmount": 99,
  "currency": "AUD",
  "externalReference": "2233445566",
  "refundReason": "Client refund request",
  "statusMessage": "Deposit refunded",
  "refundedAt": "2021-03-03T03:28:43.936Z",
  "clearedAt": "2021-03-03T03:25:12.792Z",
  "recipient": {
    "accountName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387"
  },
    "sender": {
    "accountName": "ACME Inc",
    "companyName": "ACME Inc",
    "bankName": "Bank ACME",
  },
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}
```

</details>

**Withdrawals**

<details>

<summary>withdrawal_initiated</summary>

```json
{
  "event": "withdrawal_initiated",
  "id": "51711af8c078ba061f623531",
  "amount": 2000,
  "totalFee": 1,
  "currency": "AUD",
  "externalId": "12344321",
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}
```

</details>

<details>

<summary>withdrawal_reviewing</summary>

```json
{
  "event": "withdrawal_reviewing",
  "id": "51711af8c078ba061f623531",
  "amount": 2000,
  "totalFee": 1,
  "currency": "AUD",
  "statusMessage": "Awaiting manual compliance"
  "externalId": "12344321",
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}
```

</details>

<details>

<summary>withdrawal_completed</summary>

```json
{
  "event": "withdrawal_completed",
  "id": "51711af8c078ba061f623531",
  "amount": 2000,
  "totalFee": 1,
  "currency": "AUD",
  "externalId": "12344321",
  "statusMessage": "Transaction Confirmed",
  "clearedAt": "2021-03-03T03:25:12.792Z",
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}
```

</details>

<details>

<summary>withdrawal_failed</summary>

```json
{
  "event": "withdrawal_failed",
  "id": "51711af8c078ba061f623531",
  "amount": 2000,
  "totalFee": 1,
  "currency": "AUD",
  "externalId": "12344321",
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}
```

</details>

<details>

<summary>withdrawal_refunded</summary>

```json
{
  "event": "withdrawal_refunded",
  "id": "51711af8c078ba061f623531",
  "amount": 2000,
  "totalFee": 1,
  "refundAmount": 2000,
  "currency": "AUD",
  "externalId": "12344321",
  "refundReason": "No account or incorrect account number",
  "statusMessage": "Payout reversal",
  "refundedAt": "2021-03-04T15:21:11.920Z",
  "clearedAt": "2021-03-03T03:25:12.792Z",
  "recipient": {
    "displayName": "John Smith",
    "bsb": "012620",
    "accountNo": "89900998"
  },
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "John Smith",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}
```

</details>

<details>

<summary>withdrawal_cancelled</summary>

```json
{
  "event": "withdrawal_cancelled",
  "id": "51711af8c078ba061f623531",
  "amount": 2000,
  "totalFee": 1,
  "currency": "AUD",
  "externalId": "12344321",
  "statusMessage": "Withdrawal cancelled",
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}
```

</details>

**Payments**

<details>

<summary>currency_converted</summary>

```javascript
{
  "event": "currency_converted",
  "id": "60711af8c078ba061f623531",
  "fromAmount": 1000,
  "fromCurrency": "AUD",
  "toAmount": 411.04,
  "toCurrency": "EUR",
  "externalId": "12344321"
}
```

</details>

<details>

<summary>payment_complete</summary>

```javascript
{
  "event": "payment_complete",
  "id": "60711af8c078ba061f623531",
  "fromAmount": 1000,
  "fromCurrency": "AUD",
  "toAmount": 411.04,
  "toCurrency": "EUR",
  "externalId": "12344321"
}
```

</details>

<details>

<summary>payment_failed</summary>

```javascript
{
  "event": "payment_failed",
  "id": "60711af8c078ba061f623531",
  "fromAmount": 1000,
  "fromCurrency": "AUD",
  "toAmount": 411.04,
  "toCurrency": "EUR",
  "externalId": "12344321"
}
```

</details>

<details>

<summary>payment_cancelled</summary>

```javascript
{
  "event": "payment_cancelled",
  "id": "60711af8c078ba061f623531",
  "fromAmount": 1000,
  "fromCurrency": "AUD",
  "toAmount": 411.04,
  "toCurrency": "EUR",
  "externalId": "12344321"
}
```

</details>

<details>

<summary>payment_created</summary>

```javascript
{
  "event": "payment_created",
  "id": "60711af8c078ba061f623531",
  "fromAmount": 3500,
  "fromCurrency": "EUR",
  "toAmount": 2501.94,
  "toCurrency": "AUD",
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}
```

</details>

