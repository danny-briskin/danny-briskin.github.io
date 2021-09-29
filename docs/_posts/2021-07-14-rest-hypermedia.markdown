---
layout: post
title: "Creating of self-exploring REST API"
date:   2021-07-14 14:40:30 -0400
categories: REST 
tags: REST API Hypermedia
---
![](/images/rest_hypermedia.jpg)

Danny Briskin, QA Consultants Senior Automation Engineer


# The issue
Sometimes even tons of [documentation](https://danny-briskin.github.io/api/testing/2021/07/13/api-documentation-importance.html) cannot make usage of REST requests more reasonable and sophisticated. Large systems with a complex landscape are usually covered with a lot of endpoints and navigation through them can be a headache.

# HATEOAS/HAL
There is no hate :), it is just an [approach](https://en.wikipedia.org/wiki/HATEOAS)/[HAL](https://en.wikipedia.org/wiki/Hypertext_Application_Language) to ease the usage and navigation between different endpoints. The idea of HATEOAS is to include in the response some hints or possible ways to discover more data using other endpoints. And HAL is a language to express that idea.
For example, you have just collected information about payment number 12:
{% highlight bash %}
http://localhost:8090/api/payments/12
{% endhighlight %}
Usually, you expect REST response received will contain information about the payment, like this:
{% highlight json %}
{
    "customerNumber" : 3,
    "paymentDate": "2021-08-05T10:45:10.25",
    "paymentAmount": 10.68,
    "channel": "Channel #26"
}
{% endhighlight %}
Well, that's not a bad result of course. But what if you need to look for information about the customer? Or about all payments of that customer? That's not a problem when you already have all the endpoints needed. It will be quite simple to construct new requests, run it and interpret results. Or not?
What if you don't know? What if the information needed for the next request should be preprocessed beforehand?
Let's enrich the response with some additional information (that can be easily found during response preparation). This will increase the size of response but will make it much more informative.
{% highlight json %}
{
    "customerNumber" : 3,
    "paymentDate": "2021-08-05T10:45:10.25",
    "paymentAmount": 10.68,
    "channel": "Channel #26",
    "_links": {
        "self": {
            "href": "http://localhost:8090/api/customers/3"
        },
        "customer": {
            "href": "http://localhost:8090/api/customers/3",
        },
        "paymentList": {
            "href": "http://localhost:8090/api/customers/3",
        }
    }
}
{% endhighlight %}
As you can see, all "links" (i.e., endpoints) are already constructed with all parameters. There is an endpoint to "self" to not forget how we got to this point, an endpoint to customer of this payment, an endpoint to all payments of that customer. 

In other words, the response to first endpoint "payments" contains suggestions of possible data discovery, or a set of endpoints where user can possibly go after that one. In the same way, for "customers" endpoint, there will be endpoints for customer's payments.

Using this type of responses one can construct a flexible user interface to navigate through data provided by this web service. See [HAL Explorer](https://github.com/toedter/hal-explorer) for example. Buttons are created on-the-fly from response HAL links.
![HAL explorer UI](/images/hal_explorer_04.png)

# Summary
With the raise of [Microservices architecture approach](https://en.wikipedia.org/wiki/Microservices) in software development, the importance of API correctness (and API testing, of course) becomes increasingly vital. Abundance of endpoints in a typical IT landscape makes it difficult to explore and navigate through data. HATEOAS/HAL approach requires a little additional work but can greatly increase the value of services in terms of usability and testability.