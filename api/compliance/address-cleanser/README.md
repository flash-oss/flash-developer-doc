---
description: Validate and standardise physical addresses
---

# Address Cleanser

Address Cleanser is a data-standardisation and geocoding utility designed to ensure physical addresses exist and are correctly formatted. It standardises a supplied address against multiple geocoding providers and returns a suggested recommendation together with a score.

It helps you:

* Validate and standardise addresses before processing payments
* Reduce failed deliveries due to incorrect or unrecognised addresses
* Support data integrity and screening accuracy by standardising physical addresses

{% hint style="info" %}
The feature is also available through the [Flash Connect](https://connect.flash-payments.com/) portal. You can cleanse addresses directly from the UI, view your request history, and monitor usage statistics without writing any code. This can be useful for getting familiar with the feature before integrating it into your application via the API.
{% endhint %}

You can cleanse an address via the [`cleanseAddress`](cleanse-an-address.md) mutation or retrieve past requests via the [`addressCleanserRequest`](query-address-cleanser-requests.md) and [`addressCleanserRequests`](query-address-cleanser-requests.md) queries.

Unlike Adverse Media Search, address cleansing is synchronous — the result is returned in the mutation response. There is no background processing and no webhooks.

**Recommendation and score**

The result is an estimate, not a definitive check. Each cleansed address comes with:

| Field            | Description                                                                                                                                                                                                 |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `recommendation` | <p><code>accept</code> — suggests the result can be accepted as-is. <br><code>review</code> — suggests it should be reviewed before it is used. <br><code>flag</code> — suggests it should be rejected.</p> |
| `score`          | From 0 to 100. A higher value indicates a closer match. Can be null.                                                                                                                                        |

**Pricing**

Address Cleanser is charged per request — 1,000 requests per month are included free of charge. To learn more about pricing, API access, or to enable Address Cleanser for your organisation, reach out to our team.

{% hint style="warning" %}
**Regulatory disclaimer**

**Identity Verification:** This tool does not verify that a specific individual, company, or ultimate beneficial owner is legally associated with, or resides at, the validated address.

**Compliance Obligations:** Use of this tool does not satisfy an AUSTRAC reporting entity's Know Your Customer (KYC) or Customer Due Diligence (CDD) obligations under the AML/CTF Act 2006.

**Data Retention:** To comply with statutory 7-year record-keeping requirements, users should ensure their systems retain the original, raw address string inputted by the participant alongside any cleansed data outputs.
{% endhint %}
