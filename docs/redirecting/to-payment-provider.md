---
layout: default
title: To payment provider
parent: Redirecting
nav_order: 1
---

# Redirecting to payment provider
{: .no_toc }

## Redirect straight to payment provider

In some cases, it might be useful to have a user skip our transaction page and go straight to the payment provider. This can be achieved by passing a couple of arguments to the url of our landing page.
### Bills
Here is an example of such a redirect URL:
```
https://transaction.acceptemail.com/Landing?id=7f5f945f-1965-4acc-b4cf-8a36510c0ec6&detail=true&paymentMethod=c7a8c460-e5e1-404e-a8c4-7fe5b27b48f2&subPaymentMethod=INGBNL2A&redirect=true
```
`&redirect=true` tells the page to redirect the user to the payment provider.

`&paymentMethod=c7a8c460-e5e1-404e-a8c4-7fe5b27b48f2` tells the page which payment provider.

`&subPaymentMethod=INGBNL2A` tells the page the subpaymentmethod (e.g. for iDeal, which bank).

The id's of the paymentmethod and subpaymentmethod can be found through the [PaymentMethods](https://api.acceptemail.com/swagger/ui/index#!/Bill/Bill_GetPaymentMethods) API call. See below for an example.

If an amountscheme (open or list) is used for the transaction, this can be passed by adding the amount in &amount=1500 where the amount is noted in cents.

### Mandates

Redirecting for mandates works in the same manner as bills. For the fields a user can fill in the mandate page, a number of arguments for the url are possible:

`&SequenceType= Oneff/Recurring` (Identifies the underlying transaction sequence.)

`&CollectionAmount=` Amount in cents (Fixed amount to be collected from the debtor’s account.)

`&MaximumAmount=` Amount in cents (Maximum amount that can be collected from the debtor’s account.)

`&AmountType=` Open/Fixed/Maximum (The type of the amount to be collected.)

`&ToDate=` DateTime (The date until which the mandate is valid. Only for recurring.) 


## Getting the payment methods for a bill
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