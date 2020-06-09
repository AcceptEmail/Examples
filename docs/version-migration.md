---
layout: default
title: Version migration
nav_order: 10
---

# Version migration
{: .no_toc }

## Migration from v1 to v2

The main difference between v1 and v2 of our REST API are that you can now opt for synchronous sending instead of asynchronous sendinghronous-vs-asynchronous and additional data is now set as RecordData using a standard JSON object instead of as EmailData using key-value pairs.
Below is a full list of changes and steps to be taken to migrate


<a id="synchronous-vs-asynchronous"></a>

### Bulk sending

If you are using our API for bulk sending of emails or text messages, the asynchronous option would still be the most efficient way of sending. You can keep using the asynchrous option by changing your POST endpoint from /v1/Bill to /v2/Bill/async .

### Inline use, chat & portals

If you are using our transactions in chats, chatbots, or to redirect users from your portal to our transaction page, it might be more convenient to use our synchronous POST Bill. You can use the same request as /v1/Bill but POST it to /v2/Bill. In this case, instead of just getting the ATID in the response, the response will contain everything you would use GET Bill to get in the asynchronous situation. Keep in mind that the synchronous option will take longer to respond than the asynchronous call.

## EmailData to RecordData

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
	{
	  "key1": "value1",
	  "key2": "value2"
	}
```


<a id="changed-status-code"></a>
## Changed HTTP status code

In version one, POST Bill returned a response with the HTTP code 201 (Created), but since the call is asynchronous, using the code 202 (Accepted) makes more sense. In version 2 we made sure to use 202 (Accepted) for POST /v2/Bill/async and 201 (Created) for POST /v2/Bill.
