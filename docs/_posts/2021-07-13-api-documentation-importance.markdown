---
layout: post
title: "The importance of API documentation for testing"
date:   2021-07-13 14:40:30 -0400
categories: API testing 
tags: REST API Swagger
---
![](/images/api_documentation.jpg)

Danny Briskin, QA Consultants Senior Automation Engineer

# The issue
API development and API testing are well-known things in the modern world of computer technologies. There are a lot of tools, applications, methodologies and best approaches for API testing, from well-known [Postman](https://www.postman.com/) and [SOAPUI](http://soapui.org/) to a bunch of smaller projects and plugins in development environments.
But sometimes, QA engineers are faced with certain difficulties when they need to test an API.
Contemporary [Agile approach](https://en.wikipedia.org/wiki/Agile_software_development) means that API is under constant development during testing phases. And the last thing developers pay attention to - is documentation. Sad but true, there are a lot of projects without any useful documentation at all. Many of them are lack of up-to-date documentation.
This raises the question: how to test it if you don't know where to start?

# In best practices we trust
When we deal with [SOAP-WSDL](https://www.w3.org/TR/wsdl.html) API there should be a ".wsdl" file where tester can find a lot of useful information related to available endpoints and parameters. In case of [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) API, the only thing we can trust is [REST naming convention](https://restfulapi.net/resource-naming/) (Google has [the same](https://cloud.google.com/blog/products/api-management/restful-api-design-nouns-are-good-verbs-are-bad)), the only issue there: we don't know where to start. Well, if there is a **.../customers** endpoint, highly likely there will be **customer** that supports **customer/{n}**. But not for sure.
And, of course, using [HATEOAS](https://en.wikipedia.org/wiki/HATEOAS)/[HAL](https://en.wikipedia.org/wiki/Hypertext_Application_Language) approach can greatly increase our knowledge of API (see [this article](https://danny-briskin.github.io/automation/testing/2021/07/14/rest-hypermedia.html) for more information)

# Documentation makes us happy
Sure thing, it is not easy for a developer of complex API to maintain documentation full and up to date. Especially manually. Good thing, there are a lot of libraries that can ease a burden. Moreover, it can create not only documentation but an UI for users to test it in a real environment.
Let me introduce two projects that can transform your regular API to a perfectly documented project:
- Swagger. [Swagger-UI](https://swagger.io/tools/swagger-ui/) is a tool for visualization and run REST requests. There are implementations of Swagger for different programming languages, including Java and Python. With a bit of configuration in your software, you will be able to see endpoints
![Swagger endpoints](/images/swagger_ui_01.png)

Possible errors and their descriptions
![Swagger endpoints](/images/swagger_ui_02.png)

and run your own requests
![Swagger endpoints](/images/swagger_ui_03.png)

- HAL Explorer. This [project](https://github.com/toedter/hal-explorer) can be easily integrated into your project that uses HAL Hypermedia. In case of [Spring Data Rest](https://spring.io/projects/spring-data-rest) you don't even need to do anything to configure it. It works out the box.
It will discover all available endpoints ![HAL explorer available endpoints](/images/hal_explorer_01.png) 

and you can run any request with parameters
![HAL explorer result of execution](/images/hal_explorer_02.png) 

even creation of request body is interactive
![HAL explorer UI for POST request](/images/hal_explorer_03.png) 

# Summary
Automatic documentation can greatly increase the speed of testing an API because of:
- Developer doesn't need to spend a lot of time to update current documentation
- QA engineer gets to know with API faster
- Exploration of API can be done easily and much faster that leads to earlier defects detection