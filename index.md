---
layout: default
---

AcceptEasy is a cloudbased service that enables online, mobile and social payments, mandates and verifications. This page details the integration with our REST API.


<a id="table-of-contents"></a>
# [Table of Contents](#table-of-contents)
1. [Documentation](#rest-api)
2. [Obtaining the API Keys](#obtaining-api-keys)
	1. [Rotating the API Keys](#rotating-api-keys)
3. [Use cases](#use-cases)
	1. [Create link for use inline, chat or portal](#inline-chat-portal)
	2. [Bulk sending of Bills through email, text or both](#bulk-sending)
	3. [Receiving webhooks for bounces, asynchronous creation and payment status](#receive-webhooks)
  4. [Realtime and robust payment status updates](#realtime-robust-paymentstatus)
	5. [Searching for bills previously sent to a client](#search-bills-client)
	6. [Getting the payment methods for a bill](#getting-payment-methods)
	7. [Redirect straight to payment provider](#redirecting)
	8. [Redirecting after payment](#redirect-after-payment)
4. [Migration from v1 to v2](#migration-v1-to-v2)
	1. [Synchronous vs Asynchronous](#synchronous-vs-asynchronous)
	2. [EmailData to RecordData](#emaildata-to-recorddata)
	3. [Changed HTTP status code](#changed-status-code)
5. [SOAP API](#soap-api)

For more information regarding our services, check our website: [https://www.accepteasy.com](https://www.accepteasy.com).

For technical support regarding our integrations, please contact [support@acceptemail.com](mailto:support@acceptemail.com).

<a id="rest-api"></a>
# [Documentation](#rest-api)

The Swagger documentation for the API can be found here: [https://api.acceptemail.com/swagger/ui/index#!/Bill/Bill_Post](https://api.acceptemail.com/swagger/ui/index#!/Bill/Bill_Post).
[![SwaggerDocs](/assets/SwaggerLogo.png)](https://api.acceptemail.com/swagger/ui/index#!/Bill/Bill_Post)

<a id="obtaining-api-keys"></a>
## [Obtaining the API Keys](#obtaining-api-keys)

In order to start developing with our REST API, you will need an API key

Log into the application and select Settings under the Account menu-tab (your account needs Admin or Settings-rights to access this).

At the REST API Keys Settings, enter a name and keys (or let the app generate the keys)
<div class='embed-container'><iframe src='https://player.vimeo.com/video/255000672' frameborder='0' webkitAllowFullScreen mozallowfullscreen allowFullScreen></iframe></div>


<a id="rotating-api-keys"></a>
### [Rotating the API Keys](#obtaining-api-keys)
Both keys will work for authentication, and you can renew them one by one. This enables you to rotate the keys without downtime if you want to change them. You can use the secondary key as you renew the primary.

<div class='embed-container'><iframe src='https://player.vimeo.com/video/254999563' frameborder='0' webkitAllowFullScreen mozallowfullscreen allowFullScreen></iframe></div>

To test the keys, you can use them to log in on the top right corner of the Swagger documentation: : [https://api.acceptemail.com/swagger/ui/index#!/Bill/Bill_Post](https://api.acceptemail.com/swagger/ui/index#!/Bill/Bill_Post)

<a id="use-cases"></a>
## [Use cases](#use-cases)

<a id="inline-chat-portal"></a>
### [Create link for use inline, chat or portal](#inline-chat-portal)
When using AcceptEmail transactions in chat (for agents or chatbots), or when forwarding users to our transaction pages from your portal, only minimal information needs to be sent to our API. It is easiest to use the synchronous version of POST Bill, so that you know the transaction page is available the same instant you get the response.

For instance, to create a transaction for a payment of 12,99 the following JSON object can be POSTed to /v2/Bill:
```
{
  "PaymentReference": "123456",
  "Description": "Payment from Chat",
  "Amount": 1299,
  "ExpiryDate": "2018-04-01T09:00:00Z",
}
```

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
### [Bulk sending of Bills through email, text or both](#bulk-sending)
When sending AcceptEmail transactions through email, it can be useful to add some additional information to your calls, so that this data can be used in the email- or text templates. For instance, it's very common to provide a recipient salutation and address lines.

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


<a id="receive-webhooks"></a>
### [Receiving webhooks for bounces, asynchronous creation and payment status](#receive-webhooks)

Webhooks can be used to get realtime feedback on the status of AcceptEmail transactions. Some examples of our webhooks:

Bounce: 
```
{
  "ATID": "120b6125-fdfa-4124-a08c-dbf63f38e162",
  "ERROR": null,
  "PaymentReference": "123456",
  "SRRID": "r180205114728321",
  "STATUS": "Bounced"
}
```

Creation success: 
```
{
  "ATID": "120b6125-fdfa-4124-a08c-dbf63f38e162",
  "ERROR": null,
  "PaymentReference": "123456",
  "SRRID": "r180205114728321",
  "STATUS": "CreationSucceeded"
}
```

Creation error: 
```
{
  "ATID": "120b6125-fdfa-4124-a08c-dbf63f38e162",
  "ERROR": "APP0225 - Expiry date cannot exceed 1 year from now.",
  "PaymentReference": "123456",
  "SRRID": "r180205114728321",
  "STATUS": "CreationFailed"
}
```
<a id="receive-webhooks-payment"></a>
Payment made: 
```
{
  "ATID": "120b6125-fdfa-4124-a08c-dbf63f38e162",
  "ERROR": null,
  "PaymentReference": "123456",
  "SRRID": "r180205114728321",
  "STATUS": "Paid"
}
```

These webhooks will be sent to a HTTPS endpoint that can be set in your account. 

<div class='embed-container'><iframe src='https://player.vimeo.com/video/254997339' frameborder='0' webkitAllowFullScreen mozallowfullscreen allowFullScreen></iframe></div>

Upon receiving a webhook, for instance, for payment, you can use GET /v2/Bill/[ATID], to fetch additional information such as the account holder name, or account number for the account that completed the payment.

<a id="realtime-robust-paymentstatus"></a>

### [Realtime and robust payment status updates](#realtime-robust-paymentstatus)

To receive realtime status updates about payments, we can push a webhook to a HTTPS endpoint, [the payment example can be found above](#receive-webhooks-payment).

However, an endpoint can always temporarily become unavailable, so we advice a fallback to check the payment statusses. There are 3 main options for this:
1. Matching the incoming funds on the bank account
This is likely already happening, if you have an existing flow of funds. One thing to note is that this can take a few days, depending on how exactly this is setup.
2. Poll GET Bill for outstanding payment
This can be done every hour, every day, or using any other timeframe that is appropriate for the scenario. Depending on the number of concurrent outstanding bills, this can result in a high number of API calls, which is something to take into consideration when choosing the polling frequency.
3. Use the Search/Payment API call (available from February 26 2019)
Using the Search/Payment API call, for instance every hour, you will receive all payments made using a single API call. This would be our advised  way of making sure all payments are known in your systems. We would also suggest having some overlap in the time frame, if you are looking at payments of the last hour, to account for transactions that might have been outstanding during the turn of the hours. 
An example of what that might look like:
```
https://api.acceptemail.com/v2/Search/Payment?paymentDateFrom=2019-01-15T09%3A00%3A00Z&paymentDateTo=2019-01-16T09%3A00%3A00Z&type=Bills
```
More information about the available attributes and the exact response can be found in the [SwaggerDocs](https://api-acp.acceptemail.com/swagger/ui/index#!/Search/Search_SearchPayment)




<a id="search-bills-client"></a>
### [Searching for bills previously sent to a client](#search-bills-client)
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

<a id="getting-payment-methods"></a>
### [Getting the payment methods for a bill](#getting-payment-methods)
To get a list of all possible paymentmethods for a bill you can do the following call:
```
https://api.acceptemail.com/v2/Bill/7f5f945f-1965-4acc-b4cf-8a36510c0ec6/PaymentMethods
```

Which will return something like the following:
```
[
    {
        "PaymentLogo": "https://transaction.acceptemail.com/Content/Images/PayPal_30x26.png",
        "PaymentMethodId": "8df82143-a825-4c46-a1f2-3da849760093",
        "PaymentMethodName": "PayPal"
    },
    {
        "PaymentLogo": "https://transaction.acceptemail.com/Content/Images/iDEAL_40x26.png",
        "PaymentMethodId": "c7a8c460-e5e1-404e-a8c4-7fe5b27b48f2",
        "PaymentMethodName": "iDEAL",
        "SubPaymentMethods": [
            {
                "SubPaymentMethodId": "ABNANL2A",
                "SubPaymentMethodName": "ABN AMRO"
            },
            {
                "SubPaymentMethodId": "ASNBNL21",
                "SubPaymentMethodName": "ASN Bank"
            },
            {
                "SubPaymentMethodId": "BUNQNL2A",
                "SubPaymentMethodName": "bunq"
            },
            {
                "SubPaymentMethodId": "HANDNL2A",
                "SubPaymentMethodName": "Handelsbanken"
            },
            {
                "SubPaymentMethodId": "INGBNL2A",
                "SubPaymentMethodName": "ING"
            },
            {
                "SubPaymentMethodId": "KNABNL2H",
                "SubPaymentMethodName": "Knab"
            },
            {
                "SubPaymentMethodId": "MOYONL21",
                "SubPaymentMethodName": "Moneyou"
            },
            {
                "SubPaymentMethodId": "RABONL2U",
                "SubPaymentMethodName": "Rabobank"
            },
            {
                "SubPaymentMethodId": "RBRBNL21",
                "SubPaymentMethodName": "RegioBank"
            },
            {
                "SubPaymentMethodId": "SNSBNL2A",
                "SubPaymentMethodName": "SNS"
            },
            {
                "SubPaymentMethodId": "TRIONL2U",
                "SubPaymentMethodName": "Triodos Bank"
            },
            {
                "SubPaymentMethodId": "FVLBNL22",
                "SubPaymentMethodName": "van Lanschot"
            }
        ]
    }
]
```
This example has two paymentmethods, iDeal and PayPal. For the iDeal paymethod, it also lists SubPaymentMethods, which are usually shown as a dropdown-selection on the landingpage.

<a id="redirecting"></a>
## [Redirect straight to payment provider](#redirecting)

In some cases, it might be useful to have a user skip our transaction page and go straight to the payment provider. This can be achieved by passing a couple of arguments to the url of our landing page.
### Bills
Here is an example of such a redirect URL:
```
https://transaction.acceptemail.com/Landing?id=7f5f945f-1965-4acc-b4cf-8a36510c0ec6&detail=true&paymentMethod=c7a8c460-e5e1-404e-a8c4-7fe5b27b48f2&subPaymentMethod=INGBNL2A&redirect=true
```
The &redirect=true tells the page to redirect the user to the payment provider.

The &paymentMethod=c7a8c460-e5e1-404e-a8c4-7fe5b27b48f2 tells the page which payment provider.

The &subPaymentMethod=INGBNL2A tells the page the subpaymentmethod (e.g. for iDeal, which bank).

The id's of the paymentmethod and subpaymentmethod can be found through the [PaymentMethods](https://api.acceptemail.com/swagger/ui/index#!/Bill/Bill_GetPaymentMethods) API call. See also above.

If an amountscheme (open or list) is used for the transaction, this can be passed by adding the amount in &amount=1500 where the amount is noted in cents.

### Mandates

Redirecting for mandates works in the same manner as bills. For the fields a user can fill in the mandate page, a number of arguments for the url are possible:

&SequenceType= Oneff/Recurring (Identifies the underlying transaction sequence.)

&CollectionAmount= Amount in cents (Fixed amount to be collected from the debtor’s account.)

&MaximumAmount= Amount in cents (Maximum amount that can be collected from the debtor’s account.)

&AmountType= Open/Fixed/Maximum (The type of the amount to be collected.)

&ToDate= DateTime (The date until which the mandate is valid. Only for recurring.) 

<a id="redirect-after-payment"></a>
## [Redirecting after payment](#redirect-after-payment)
Once a user has made a payment at their paymentprovider, they'll be redirected to the AcceptEasy landing page. In some cases you might want to redirect the user to another page or app.

To set this up. In the account, add a Result Banner, under the Templates menu. You can set links for both completed payments and unfinished (failed or cancelled) payments. After this, add the Result Banner to the AE Template and make sure to add the amount of seconds before the redirect should take place (enter 0 for immediate redirect).
If you want each record to have unique redirect URL's, you can add this through the API by adding the following:
```
  "Extras":{
  	"ReturnBannerOpenURL":"http://www.example.com",
  	"ReturnBannerPaidURL":"http://www.example.com"
  }
```
You can also add app-urls in both the Result banner and the record-specific URL's to be able to redirect a user to your mobile app.

<a id="migration-v1-to-v2"></a>
## [Migration from v1 to v2](#migration-v1-to-v2)

The main difference between v1 and v2 of our REST API are that you can now opt for [synchronous sending instead of asynchronous sending](#synchronous-vs-asynchronous), and additional data is now set as [RecordData using a standard JSON object instead of as EmailData using key-value pairs](#emaildata-to-recorddata). We also [changed the HTTP status code](#changed-status-code) for the response of Bill/async from 201 (Created) to 202 (Accepted).


<a id="synchronous-vs-asynchronous"></a>
### [Synchronous vs Asynchronous](#synchronous-vs-asynchronous)

#### Bulk sending

If you are using our API for bulk sending of emails or text messages, the asynchronous option would still be the most efficient way of sending. You can keep using the asynchrous option by changing your POST endpoint from /v1/Bill to /v2/Bill/async .

#### Inline use, chat & portals

If you are using our transactions in chats, chatbots, or to redirect users from your portal to our transaction page, it might be more convenient to use our synchronous POST Bill. You can use the same request as /v1/Bill but POST it to /v2/Bill. In this case, instead of just getting the ATID in the response, the response will contain everything you would use GET Bill to get in the asynchronous situation. Keep in mind that the synchronous option will take longer to respond than the asynchronous call.

<a id="emaildata-to-recorddata"></a>
### [EmailData to RecordData](#emaildata-to-recorddata)

Instead of using key-value pairs like EmailData in v1, RecordData uses a standard JSON Object. If you are using EmailData in v1, you will have to rename to RecordData and restructure the way the attributes are sent. So:

```
"EmailData": 
[
    {
      "key": "key1",
      "value": "value1"
    },
    {
      "key": "key2",
      "value": "value2"
    }
  ]
```

Will have to change to:

```
"RecordData": 
[
	{
	  "key1": "value1",
	  "key2": "value2"
	}
]
```


<a id="changed-status-code"></a>
### [Changed HTTP status code](#changed-status-code)

In version one, POST Bill returned a response with the HTTP code 201 (Created), but since the call is asynchronous, using the code 202 (Accepted) makes more sense. In version 2 we made sure to use 202 (Accepted) for POST /v2/Bill/async and 201 (Created) for POST /v2/Bill.

<a id="soap-api"></a>
# [SOAP API](#soap-api)

We also have a SOAP API available. To obtain the Integration Guide for the SOAP API, please contact us at [support@accepteasy.com](mailto:support@accepteasy.com)
