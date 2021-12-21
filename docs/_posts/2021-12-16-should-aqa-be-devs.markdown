---
layout: post
title: "Should QA automation engineers be software developers first? "
date:   2021-12-16 14:40:30 -0400
categories: documentation 
tags: AQA developer SDET
---
![](/images/flexibility_aspectj.jpg)

Danny Briskin, QA Consultants Senior Automation Engineer


# The issue

A typical way of becoming an automation quality assurance engineer (or, as it is called sometimes - a "software developer in test", SDET) is in enhancing skills of a manual QA in programming area. Sometimes, professionals from other IT-related fields want to change professions: system administrators, network, support and operation engineers. But software developers do that rarely. They think about it as a kind of "relegation to a lower league". 

Thus, a vast majority of new Automation Quality Assurance (AQA) engineers lack regular software development studies and practice. 

Is it bad or good? 

On the one hand, they look to the same issues from another angle than developers and, thus, can solve it in a separate way. On the other hand, their solutions could be less efficient, non-reusable and hardly maintainable in comparison to systems created by software developers. 

But do they need to? 

If you create a test automation suite, it is supposed to be run on a single system, single project, single set of data. Why spend time and effort to analysis and architecture the system, to create something more complex? 

Time is money, ~~fire-and-forget~~ test-and-run, right? 

# Large teams

The main issue about simple and straightforward code you produce arises when you are not the only one AQA in the project. If you perform along with other AQAs on the same test framework, the code will be duplicated more than once, and time saved for prior analysis and architecture will be wasted. Then, you will create pieces of code that will contradict each other, and finally, some methods will mutually block other (normal) routine processes. 

When it turns out that test framework has reached its performance capacity you will be definitely faced the need of scaling, but your spaghetti code is neither understandable nor be readable. 

# Similar projects

Think wider! Most automation test projects are more or less about the same. It is either Web, UI, API or mobile testing. And, if you tested one Web application, you tested them all. The same is for other types. 

At least some part of your code could be reused in case you created it wisely. In that case you will spend less time starting your next project. That means you will be able to spend more time researching more critical issues. 

# Reusable framework

Once you applied the same framework to several projects, it might become so sophisticated that you will only need to change just several parameters in order to run it against a new system under test. That means, the only thing you need to create for a completely independent test system is a User Interface. All major test automation systems were developed exactly in that way. 


# What are "must have" requirements for automation test framework

* Code readability.

All code should be easily read by other AQAs (and by "you-one-month-later" of course). Please avoid spaghetti code, follow code guidelines of programming language you use, add comments and documentation wherever possible. 

* Project maintainability. 

Use a version control system. Make all changes easily reversable. Be able to support several versions of your code at a time ( i.e., support development in several branches). Use continuous integration software to automate test launches and reporting of the test framework. Maintain proper logging during test execution to ensure ability of aftermath research. 

* Scalability 

Split your framework into parts when it is possible. Containerize fewer changing parts and scale it with help of containerization software. Always categorize/tag and prioritize your test cases to be able to run a certain number of test cases in a time or batch. 

# Summary

Thinking as a developer is not mandatory for an AQA engineer, but an extremely useful skill. It enables you to create reusable, maintainable and scalable test frameworks and apply it to different systems. Enhancing development skills enables you to understand the code of the system under test and talk to developers in their language. But the top winner is you! Creation of comprehensive and flexible software is always an aesthetical pleasure. 