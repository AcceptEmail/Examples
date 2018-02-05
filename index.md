---
layout: default
---

AcceptEasy is a cloudbased service that enables online, mobile and social payments, mandates and verifications. This page details the integration with our REST API.


<a id="table-of-contents"></a>
# [Table of Contents](#table-of-contents)
1. [REST API](#rest-api)
2. [Obtaining the API Keys](#obtaining-api-keys)
3. [Migration from v1 to v2](#migration-v1-to-v2)
	1. [Synchronous vs Asynchronous](#synchronous-vs-asynchronous)
	2. [EmailData to RecordData](#emaildata-to-recorddata)
	3. [Changed HTTP status code](#changed-status-code)
4. [SOAP API](#soap-api)

For more information regarding our services, check our website: [http://www.accepteasy.com](http://www.accepteasy.com)

For technical support regarding our integrations, please contact [support@acceptemail.com](mailto:support@acceptemail.com)

<a id="rest-api"></a>
# [REST API](#rest-api)

The documentation for the API can be found here: [https://api.acceptemail.com/swagger/ui/index#!/Bill/Bill_Get](https://api.acceptemail.com/swagger/ui/index#!/Bill/Bill_Get)

<a id="obtaining-api-keys"></a>
## [Obtaining the API Keys](#obtaining-api-keys)

In order to start developing with our REST API, you will need an API key

Log into the application and select Settings under the Account menu-tab (your account needs Admin or Settings-rights to access this).

At the REST API Keys Settings, enter a name and keys (or let the app generate the keys)
![REST](assets/GenerateRestKey.gif)

<a id="migration-v1-to-v2"></a>
## [Migration from v1 to v2](#migration-v1-to-v2)

The main difference between v1 and v2 of our REST API are that you can now opt for [synchronous sending instead of asynchronous sending](#synchronous-vs-asynchronous), and additional data is now set as [RecordData using a standard JSON object instead of as EmailData using key-value pairs](#emaildata-to-recorddata). We also ]changed the HTTP status code](#changed-status-code) for the response of Bill/async from 201 (Created) to 202 (Accepted).


<a id="synchronous-vs-asynchronous"></a>
### [Synchronous vs Asynchronous](#synchronous-vs-asynchronous)

#### bulk sending

If you are using our API for bulk sending of emails or text messages, the asynchronous option would still be the most efficient way of sending. You can keep using the asynchrous option by changing your POST endpoint from /v1/Bill to /v2/Bill/async .

#### inline use, chat & portals

If you are using our transactions in chats, chatbots, or to redirect users from your portal to our transaction page, it might be more convenient to use our synchronous POST Bill. You can use the same request as /v1/Bill but POST it to /v2/Bill. In this case, instead of just getting the ATID in the response, the response will contain everything you would use GET Bill to get in the asynchronous situation.

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

We also have a SOAP API available. To obtain the Integration Guide for the SOAP API, please contact us at [support@acceptemail.com](mailto:support@acceptemail.com)