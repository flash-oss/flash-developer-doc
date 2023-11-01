---
description: CRUD queries for Institutions
---

# Institutions

### Instructing Institution

`Institution` entity is a piece of information about the party who instructed you to make the withdrawal. This data is important for compliance within Australia. This is not the sender. If there is no other institution who has instructed this withdrawal, leave this blank and your own details will be used for compliance and auditing purposes.&#x20;

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption><p>Institution explanation</p></figcaption></figure>

#### Creating institutions

* via Flash Connect (you can copy an ID of an existing Institution).&#x20;
* via API by invoking `createInstitution` mutation (coming soon)
* by providing `instructingInstitution` object into `createWithdrawal` mutation.&#x20;

{% hint style="warning" %}
Please avoid creating multiple Institutions for the same organisation because they are used for compliance reporting and subject to review.&#x20;
{% endhint %}

