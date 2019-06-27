---
layout: default
title: Search
nav_order: 6
---

# Search
{: .no_toc }


### Searching for bills previously sent to a client
Searching for bills previously sent to a client can be done by using the payment reference, or email address, depening on which information is used during the creation of the bills. An example request for searching based on payment reference would be:
```
https://api.acceptemail.com/v2/Search/Bill?paymentReference=1111222233334444
```
To which the response might look like:
```
{
  "Bills": [
    {
      "AETemplateID": "0885bc8c-808e-41de-bac6-96d4a4ecdfe0",
      "ATID": "7f5f945f-1965-4acc-b4cf-8a36510c0ec6",
      "Address": {},
      "Amount": 0.01,
      "Description": "Description",
      "PaymentReference": "1111222233334444",
      "SRRID": "r180828113646596",
      "Status": "Open",
      "ExpiryDate": "2019-07-28T09:26:40.513Z",
      "Links": {
        "TransactionURL": "https://transaction.acceptemail.com/Landing?id=7f5f945f-1965-4acc-b4cf-8a36510c0ec6&detail=true",
        "ShortURL": "http://trx.ae/X5Rff2UZzEq0z4o2UQwOxg",
        "Images": {
          "ImageURL1": "https://images.acceptemail.com/?id=7f5f945f-1965-4acc-b4cf-8a36510c0ec6&Culture=nl-NL&part=1",
          "ImageURL2": "https://images.acceptemail.com/?id=7f5f945f-1965-4acc-b4cf-8a36510c0ec6&Culture=nl-NL&part=2"
        }
      }
    }
  ],
  "SearchMetadata": {
    "CompletedIn": 0.02,
    "Count": 50,
    "Page": 1,
    "TotalNumberOfResults": 1
  }
}
```

As you can see, the response is comprised of the search results in the Bills array, and some SearchMetaData. If the number of results exceeds the "count", and several pages of results exist, a query for the next page will be included in the SearchMetadata:
```
"SearchMetadata": {
    "CompletedIn": 0.054,
    "Count": 50,
    "Page": 1,
    "TotalNumberOfResults": 2,
    "NextResults": "?count=50&page=2&paymentReference=1111222233334444"
  }
```
