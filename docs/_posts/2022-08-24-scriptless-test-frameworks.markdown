---
layout: post
title: "Scriptless and low-scripting tools in QA automation"
date:   2022-08-24 14:40:30 -0400
categories: QA automation
tags: QA automation, tools, scriptless
---
![](/images/setup_aspectj.jpg)

Danny Briskin, Senior Technical Consultant


# The issue
There was an increased number of tools in QA automation software market last time, that are designated as "Low scripting" and even "Scriptless". Is the future of QA automation engineer profession in jeopardy? Will they be replaced by tools? Can anyone, a computer literate person, create and run test automation scripts? Let's figure it out.

# Background and motivation
There's nothing new that demand for testing is growing as the number of software products grows as well. Obviously, the need for automation is also high. And the supply of highly qualified developers (a.k.a - QA automation engineers or software developers in test -SDETs) is not remarkably high because of natural reasons ([see this article](https://danny-briskin.github.io/java/2021/12/16/2021-12-16-should-aqa-be-devs.html)).
It was a reasonable move from a lot of companies to create helpful applications to ease test automation process, and thus, to lower the entry level of engineers to join QA automation team. First, they added an possibility to record-and-play test scripts with the ability to edit it. Low-scripting tools have appeared in this way. And then, some tools have switched from scripting (or coding) to building test scripts from predefined, sometimes parametrized blocks. That was the birth of scriptless tools.

# Problem is solved?
Any test script has some logic inside, even if it is quite simple. And it looks like there is no difference whether you put it as a script or drag-and-drop some boxes and check some options. But what if the logic becomes a little bit more complex? Theoretically, any complex problem can be solved like that. There is only a usability issue. 

## A real use case
A sriptless tool has basic arithmetic operations (+, -, *, /), a condition operator and a GOTO operator. There is no built-in loop. Try to calculate [CRC](https://en.wikipedia.org/wiki/Cyclic_redundancy_check) using that set of operators.

# Performance
Making a tool easy-to-use by a "not an engineer" we should be ready to pay for that in script execution performance. Something must translate "boxes" into computer-intelligible instructions and executes them. No one guarantees it will be done in the most efficient way. Most likely it will not.

# Another level of complexity
All those tools are done by software developers. That means there is a built-in logic inside a tool. The end user cannot change it, they can only "trust in" it. So, during the test process we have just another proxy layer. I.e., in order to press a button in the system under test (as a manual QA does), we call a tool, that calls another tool, that calls... finally to press a button. As more complex a tool is, the probability of a bug inside the chain of calls grows significantly.

# Software is just not created in that way!
Test automation is a kind of software development. Thus, its creation must follow some basic guidelines of development. To create a test automation script, we must:
* Imagine a model of the future application
* Figure out what data we are working with, structure it, determine entities
* Realize entities relationships, interactions are limitations on the data
* Specify data flows, create conditions and formulas
* Transform those flows into a computer-intelligible form (whether as code or in a scriptless form)
* Debug it and test 
* Optimize performance
* Maintain the application in a way that anyone else is able to figure out and be able to make changes in it

Do you still believe that any "just computer literate" person can do it with adequate quality?

# Summary
Is there is a place for "Low scripting" and "Scriptless" tools in the market? For sure there is. There are a lot of so-called "onetime test applications". When there is no need for flexibility, low chance of system under test modifications and no intention of any kind maintenance. Create, run, get results and forget. Usually, the complexity of such test applications is not extremely high. This niche can be (and is) filled with those tools. On the other hand, when the complexity of test application is medium-to-high, when there are plans of long-time maintenance, one should consider hiring a software developer, a person with a certain pattern of thinking.

