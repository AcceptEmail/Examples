---
layout: default
title: Mandates
parent: Creating a record
nav_order: 2
---

# Mandates
{: .no_toc }



### Create link for use inline, chat or portal
When using our links transactions in chat (for agents or chatbots), or when forwarding users to our transaction pages from your portal, only minimal information needs to be sent to our API. Youcan POST a JSON object like below to /v2/Mandate:
```
{
  "PaymentReference": "123456",
  "Description": "my description",
  "SequenceType": "Recurring",
  "ToDate": "2020-05-24T11:10:17.7245913Z",
  "AmountType": "Open",
  "MaximumAmount": 5400,
}
```


### Sending a mandate through email
When you want to send a mandate through email you will need to POST a JSON object like below to /v2/Mandate, which include the email address and a communication object:
```
{
  "PaymentReference": "123456",
  "Description": "my description",
  "Address": {
    "Email": "contact@accepteasy.com"
  },
  "SequenceType": "Recurring",
  "ToDate": "2020-05-24T11:10:17.7245913Z",
  "AmountType": "Open",
  "MaximumAmount": 5400,
  "Communication": [
    {
      "Channel": "Email",
      "MessageStatus": "All",
      "MandateStatus": "All"
    }
  ]
}
```

This will result in a request that gives the sender the mandate to make a one-time withdrawal of a maximum of 54 euros, before May 24th 2020.
The [Swagger docs](https://api.acceptemail.com/swagger/ui/index#!/Mandate/Mandate_Post_v2) give a full overview of the different options and statuses.

It is important to note that the PaymentReference field can be 36 characters, but most payment providers only allow a maximum of 16 characters, so we advice to keep it below that.

The Description field has a maximum of 32 characters.
