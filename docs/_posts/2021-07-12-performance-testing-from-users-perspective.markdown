---
layout: post
title: "Performance testing from users' perspective"
date:   2021-07-12 14:40:30 -0400
categories: performance testing 
tags: performance jmeter loadrunner 
---
![](/images/performance_testing.jpg)

Danny Briskin, QA Consultants Senior Automation Engineer


# The issue
A standard task of automated web site performance testing can be a bit tricky when you start to think about real users behavior. All well-known tools for performance measurement ([Microfocus LoadRunner](https://www.microfocus.com/en-us/products/loadrunner-professional/overview) or [Apache JMeter](https://jmeter.apache.org/)) works to emulate users experience on websites. **Only** emulate, not imitate! In other words, an application(performance measurement script) communicates with another application (web site) pretends to be a human being.
But application cannot emulate human reactions, feelings and considerations.
For example, if there is a button of extremely small size on a page, the script will click on it without any hesitation, while a human will spend some time to locate the button, then will try to move mouse pointer over it, misclicks it, try again and finally will click it. Script spends 0.5 seconds to click the button, while human spends 10 seconds. How can we trust the results of performance measurment?
And I am not talking about the case when the button is hidden from human's view and is "visible" for automation script (See this [article](https://danny-briskin.github.io/web/testing/2021/07/10/find-hidden-or-partially-hidden-images-on-a-web-page-using-ocr.html) if you are interesting how to work with that).

# How come?
Users' interactions with a website are done using web protocols like [HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) or [WebSocket](https://en.wikipedia.org/wiki/WebSocket). In short, some bytes are going in and out between webserver and web client (browser). Performance measurment tools use those protocols to send and receive data, omitting user layer. I.e., it considers if some bytes came from server it is automatically  acknowledged by user's side and user is ready for the next iteration (to send some bytes in response). Of course we can use timeouts in the scripts but all of them will be artificial, not representing real user behavior

# A solution?
That's begs the question - how to imitate a real user experience? Quick answer: there is no way to fully automate it currently. But one can try.
## Timeouts recordings 
A viable approach is to record several real user website interactions and use their average "timeouts" in web site elements interaction in the result performance measurment script. There are drawbacks:
- how to find adequate number of "average users" to record?
- how to measure "timeouts" and calculate average(mean?) value

## Selenium WebDriver
Another approach is to use a Selenium WebDriver to imitate users interaction. Of course, Selenium has its own limitations to emulate users actions but it is a way more "human like" than a plain HTTP protocol. Yes, it can click on "human-invisible" button too, but a button will be clicked only of page is completely loaded and that button was rendered. Selenium allows to emulate mouse movement and timeouts as well. The only thing that undermines this approach is the number of resources needed to make a reliable performance test. Each Selenium WebDriver is a little browser and it is not easy to run thousands of them.

# Grid and containers
Contemporary computing resources are growing with usage of [conteinirization](https://www.ibm.com/cloud/learn/containerization) and automatic containers management. Selenium also has such a solution: [Grid](https://www.selenium.dev/documentation/en/grid/grid_4/). But from my experience, there is another much more convenient solution to use with Selenuim WebDriver: [Selenoid](https://aerokube.com/selenoid/). JMeter has several plugins to help us to run the test: [WebDriver Components ](https://github.com/undera/jmeter-plugins-webdriver) and [Remote Driver Config](https://jmeter-plugins.org/wiki/RemoteDriverConfig/). Having set it up we can easily add a standard Selenium script right into JMeter and run it on a remote Selenoid with as much browsers as it is possible on our premises.

# Summary
Performance testing of a machine-machine interface (like API testing) is not a rocket science now. But when it comes to human-machine interactions testing, the approach cannot be the same.
Human beings are not so simple and fast as machines and their sensations, feelings, perceptions, previous experience can do tricks with performance measurment. Choosing the right tool and approach of testing is a key factor to achieve correct results.