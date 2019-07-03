---
layout: default
title: Payment status
nav_order: 5
---

# Payment status
{: .no_toc }

### Realtime and robust payment status updates

To receive realtime status updates about payments, we can push a webhook to a HTTPS endpoint.

However, an endpoint can always temporarily become unavailable, so we advice a fallback to check the payment statusses. There are 3 main options for this:

1. **Matching the incoming funds on the bank account**
This is likely already happening, if you have an existing flow of funds. One thing to note is that this can take a few days, depending on how exactly this is setup.
2. **Poll GET Bill for outstanding payment**
This can be done every hour, every day, or using any other timeframe that is appropriate for the scenario. Depending on the number of concurrent outstanding bills, this can result in a high number of API calls, which is something to take into consideration when choosing the polling frequency.
3. **Use the Search/Payment API call**
Using the Search/Payment API call, for instance every hour, you will receive all payments made using a single API call. This would be our advised  way of making sure all payments are known in your systems. We would also suggest having some overlap in the time frame, if you are looking at payments of the last hour, to account for transactions that might have been outstanding during the turn of the hours. 
An example of what that might look like:
```
https://api.acceptemail.com/v2/Search/Payment?paymentDateFrom=2019-01-15T09%3A00%3A00Z&paymentDateTo=2019-01-16T09%3A00%3A00Z&type=Bills
```
More information about the available attributes and the exact response can be found in the [SwaggerDocs](https://api.acceptemail.com/swagger/ui/index#!/Search/Search_SearchPayment)

