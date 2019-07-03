---
layout: default
title: Events
nav_order: 7
---

# Events
{: .no_toc }
Coming soon
{: .label .label-yellow }

## Getting events for a record
In the following release, it will be possible to get events for a record through the REST API with the GET Bill or GET Mandate call.

For a number of different events for a record, data will be stored and can be retrieved through the REST API. Each event will return the type and have a date/timestamp as well as the status of the record at the moment of the event.
For events directly related to the recipient, you will see the useragent and the IP-address (the last octet of the IP will always be 0, for privacy reasons).

Below is a list of the different events and examples of what the response for it will be.

### Views
When an email is sent to a user and the user opens the email, this will be registered as a view.
```
{
	"Type": "View",
	"UserAgent": "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 10.0; WOW64; Trident/7.0; Microsoft Outlook 16.0.11328; ms-office; MSOffice 16)",
	"DateTime": "2019-07-03T08:29:05.167Z",
	"Ip": "123.123.123.0",
	"Status": "Open"
}
```

### Clicks
When a user visits the transaction page (through clicking the link in the email or sms or other medium) it will be registered as a view.
```
{
	"Type": "Click",
	"UserAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.100 Safari/537.36",
	"DateTime": "2019-07-03T08:29:55.287Z",
	"Ip": "123.123.123.0",
	"Status": "Open"
}
```

### Attempt
When a user clicks the 'Pay Now' button on the transaction page (after selecting the payment method) it will be registered as an attempt.
```
{
	"Type": "Attempt",
	"UserAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.100 Safari/537.36",
	"DateTime": "2019-07-03T08:30:28.377Z",
	"PaymentMethodId": "c7a8c460-e5e1-404e-a8c4-7fe5b27b48f2",
	"PaymentMethodName": "Direct",
	"SubPaymentMethodId": "INGBNL2A",
	"Ip": "123.123.123.0",
	"Status": "Open"
}
```

### Payment
When a transaction is succesfully finished, it will be registered as a payment.
```
{
	"Type": "Payment",
	"DateTime": "2019-07-03T08:30:35.227Z",
	"PaymentMethodId": "c7a8c460-e5e1-404e-a8c4-7fe5b27b48f2",
	"PaymentMethodName": "Direct",
	"SubPaymentMethodId": "INGBNL2A",
	"Status": "Paid"
}
```