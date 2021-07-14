---
layout: post
title: "Test operations in test automation"
date:   2021-07-11 14:40:30 -0400
categories: automation testing 
tags: automation java jenkins 
---
![](/images/testops_testautomation.jpg)

Danny Briskin, QA Consultants Senior Automation Engineer


# The issue
Test automation is consistently acquiring the key role in testing of most digital-era companies
according to the latest [World Quality Report](https://www.capgemini.com/research/world-quality-report-wqr-20-21/). The need of SDET (Software Development Engineer in Test) is growing as well and the topmost skill desired by employers is "Software development". Well, it is quite obvious: test automation is a kind of programming, not a kind of testing, to my mind.

But the area of skills applying in test automation is much more larger than just software development

# "Swiss army multi-tool"
Let's take a look to a typical project of test automation. There is a system under test of course. And it will not be a surprise if there are several loosely coupled legacy systems with a complicated development lifecycle and CI/CD procedures.
It is expected from a SDET that they will create an automation script that can be used as an universal test tool in a software development project.
Sounds pretty easy, right?
But it is often forgotten, that just to start working they need:
- development infrastructure
    - computers and/or servers to workgood mornin
    - IDE, software to access servers, systems, databases, API, etc.
    - additional database(s) or some workspace in existing one
    - workaround scripts to setup local environment
- collaboration software - task management, version control, defects, etc. Could be common with developers team or a separate one

After they have started to implement tests and are ready to run it they may also need:
- test data - a renewable and up-to-date data to be used in tests without need of creation mocks
- test environment to run test artifacts (sometimes several environments)
- CI/CD system - either an existing one and/or a local one 
- reporting tool/system - to publish test reports
- test case prioritization knowledge

In case of a large company and high budgeted project, quite possible that all of this is provided. But on the other hand... Well I suppose you know, all those projects ;-)

Knowledge of all that stuff demands to be a "Swiss army multi-tool" from a regular SDET. We have a developer, database administrator, Windows/Unix power user, DevOps, test manager and a domain expert inside a single person.
Sure, there are such a specialists. But the budget may not stand their price.
Even if it can, that kind of SDET will work mostly on non-development tasks while stakeholders expect test automation to be done in the first place.

# Divide and rule
Let's leave development for developers and there are TestOps for the rest. 

Test Operations (TestOps)
: is the convergence of testing with operations and development. A discipline of efficiently managing testing processes, tools, and people to maximize delivery speed and application quality. 

It is a relatively new discipline, but it comes more and more popular. TestOps engineers (as their DevOps counterparts) may not be a highly-specialized in all those spheres of knowledge mentioned above. They have to know enough of all of that to be able to help SDETs to setup their environments.
Having done that, their job is completed. In other words, the project does not need to have TestOps assigned all the time. They can do periodic babysitting and save a lot of customer's money.
And SDETs can concentrate on their software development issues without distraction.

The process of TestOps training is almost similar to DevOps. Additionally they need to be trained test data management and test reporting tools.

# Summary
The rise of test automation led to the rise of a supplementary profession - TestOps. Those professionals can greatly increase the value of test automation and increase the speed of application delivery.
