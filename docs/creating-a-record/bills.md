---
layout: default
title: Bills
parent: Creating a record
nav_order: 1
---

# Bills
{: .no_toc }

### Create link for use inline, chat or portal
When using our links transactions in chat (for agents or chatbots), or when forwarding users to our transaction pages from your portal, only minimal information needs to be sent to our API. It is easiest to use the synchronous version of POST Bill, so that you know the transaction page is available the same instant you get the response.

For instance, to create a transaction for a payment of 12,99 the following JSON object can be POSTed to /v2/Bill:
```
{
  "PaymentReference": "123456",
  "Description": "Payment from Chat",
  "Amount": 1299,
  "ExpiryDate": "2018-04-01T09:00:00Z",
}
```

It is important to note that the PaymentReference field can be 36 characters, but most payment providers only allow a maximum of 16 characters, so we advice to keep it below that.

The Description field has a maximum of 32 characters.

From the response, you can use the ShortURL of the full TransactionURL:

```
{
  "ATID": "120b6125-fdfa-4124-a08c-dbf63f38e162",
  "SRRID": "r180205114728321",
  "AETemplateID": "4a5c25a1-787a-439c-9089-2b78914be777",
  "PaymentReference": "123456",
  "Amount": 12.99,
  "Description": "Payment from Chat",
  "ExpiryDate": "2018-04-01T09:00:00Z",
  "Status": "Open",
  "Address": {
    "Email": "",
    "PhoneNumber": ""
  },
  "Communication": [],
  "Links": {
    "TransactionURL": "https://transaction.acceptemail.com/Landing?id=120b6125-fdfa-4124-a08c-dbf63f38e162&detail=true",
    "ShortURL": "http://trx.ae/JWELEvr9JEGgjNv2PzjhYg",
    "Images": {
      "ImageURL1": "https://images.acceptemail.com/?id=120b6125-fdfa-4124-a08c-dbf63f38e162&Culture=nl-NL&part=1",
      "ImageURL2": "https://images.acceptemail.com/?id=120b6125-fdfa-4124-a08c-dbf63f38e162&Culture=nl-NL&part=2"
    }
  }
}
```

<a id="bulk-sending"></a>
### Bulk sending of Bills through email, text or both
When sending bills through email, it can be useful to add some additional information to your calls, so that this data can be used in the email- or text templates. For instance, it's very common to provide a recipient salutation and address lines.

When creating the transactions, it is possible to plan all future communications based on certain conditions. So you can choose to send the transaction through email right away, again through text a week later if it hasn't been paid yet, and finally a few days before expiry.

An example that might be posted to /v2/Bill/async would be:
```
{
  "PaymentReference": "123456",
  "Description": "MonthlyInvoice",
  "ExpiryDate": "2018-02-28T23:59:59Z",
  "Address": {
    "Email": "contact@accepteasy.com",
    "PhoneNumber": "+31 20 261 00 20"
  },
  "Amount": 1299,
  "Communication": [
    {
      "Channel": "Email",
      "Template": "initialPaymentRequest",
      "ScheduledDate": "2018-02-05T10:11:22Z",
    },
    {
      "Channel": "Text",
      "PaymentStatus": "Open",
      "Template": "preminderText",
      "ScheduledDate": "2018-02-12T15:11:22Z",
    },
    {
      "Channel": "Email",
      "PaymentStatus": "Open",
      "Template": "finalPreminder",
      "ScheduledDate": "2018-02-25T10:11:22Z",
    },
  ],
  "RecordData": {
    "RecipientDisplayName": "Arjan",
    "RecipientAddressLine1": "Keizer Karelplein 5",
    "RecipientAddressLine2": "1185 HL",
    "RecipientAddressLine3": "Amstelveen",
    "AccountNumber": "19282489"
  }
}
```

In response to this, you will receive just the ATID, that can be used with GET /v2/Bill/[ATID] to get updates to the status of the transaction.
