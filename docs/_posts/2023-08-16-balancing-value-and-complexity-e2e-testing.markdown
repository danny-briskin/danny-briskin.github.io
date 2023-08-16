---
layout: post
title: "Test Automation Scope: Balancing Value and Complexity with End-to-End Testing"
date:   2023-08-16 07:40:30 -0400
categories: testing 
tags: testing, e2e, automation
---
![](/images/hidden_images.jpg)

Danny Briskin, Quality Engineering Practice Manager


# Introduction
In the realm of Quality Assurance (QA) automation, a recurring conundrum faces Software Development Engineer in Test professionals, at the outset of each project or even within Agile Sprints: What is the optimal scope of automation? The notion of "automating everything" is a common refrain from clients; however, deciphering the precise scope of "everything" remains a challenge. The process involves a judicious selection of test types for automation, a task that extends beyond mere mechanical execution.. 

# Strategies for Determining Automation Scope
## The Simple and Direct Approach: Full Automation
This method entails mirroring manual test cases by automating each one. This approach provides comprehensive coverage, a favored aspect by project managers, and establishes a one-to-one mapping between manual and automated test suites, enhancing reporting capabilities.
Of course there is a drawback. This strategy often yields an extensive automation test suite, often marred by redundant or partially redundant test cases, leading to increased complexity and inefficiencies. Many TCs will not add any new value to the automation process and testing in general. For example, if there is a login feature in the system and you need to test a positive scenario. Obviously, you can automate it as a separate scenario but why? I'll bet 95% of your TC require a successful login, so this part will definitely be tested elsewhere.

## The Value-Based Approach: Strategic Automation
This approach involves assessing potential future costs for the development, maintenance, and execution of automated test cases. A critical metric known as the "test value" emerges from the ratio of manual testing expenses to automated testing costs. The "test value" evolves over time, becoming more favorable as development concludes and maintenance diminishes. Less intricate tests, reminiscent of component tests, can swiftly acquire value, while complex End-to-End (E2E) tests may never accrue substantial value due to elevated development and maintenance overheads.
Does it mean that one should automate only high-valued tests and never automate E2E tests?
Notably, E2E tests remain compelling for their demonstrative potential, aiding in securing top management support for test automation endeavors.

# Enhancing Test Value through Combination: Unifying Test Cases
Elevating the value of a test case can be achieved by amalgamating smaller tests. For instance, merging a login action with subsequent steps and logout functionality within the same test can optimize development, maintenance, and execution time, while manual effort increases.
This approach has drawbacks:
* Challenges arise in tracking these combined test cases within the original manual test suite, as complex one-to-many, many-to-one, or many-to-many relationships between automation and manual tests may arise.
* This approach, while increasing value, also introduces complexities by transitioning from component-level tests to broader E2E scenarios. That also increases execution time.

# End-to-End (E2E) Tests: Evaluating the Spectrum
*End-to-End (E2E) testing is a software testing technique that verifies the functionality and performance of an entire software application from start to finish by simulating real-world user scenarios.*
Reasons to **NOT** doing E2E Automation:
* E2E tests demand advanced software development skills, making implementation and maintenance daunting.
* E2E testing hinges on application state, necessitating meticulous setup and control of test data.
* The comprehensive environment required for E2E tests often involves shared components, adding intricacies.
* E2E tests encompass numerous serial steps and typically require considerable execution time.
* Execution involves interaction with user interfaces, exacerbating complexity.
* E2E tests are frequently executed later within Continuous Integration/Continuous Deployment (CI/CD) pipelines.
 
The Singular Justification for E2E Automation: Customer-Centric Assurance
* E2E tests stand as the sole category of automated tests that offer a genuine demonstration of application functionality aligned with user expectations.

# Balancing the Automation Spectrum: Making Informed Choices
Decision Framework:
* There is no universal solution, whether embracing or eschewing E2E automation. E2E tests possess undeniable value, yet their challenges are evident.
* Total exclusion of E2E tests leaves the testing suite incomplete, potentially missing critical issues only detectable through comprehensive holistic testing.

Guideline from Experience: A Balanced Approach
Based on empirical observations, a pragmatic guideline suggests allocating no more than 10% of the total number of tests and execution time to E2E tests for optimal results in most projects

# Summary

In the world of QA automation, deciding what to automate is a perpetual challenge. The "automate everything" mantra requires strategic assessment, with options like comprehensive replication of manual tests or evaluating value-based automation. End-to-End (E2E) tests, though impactful, introduce complexities such as maintenance, execution time, and UI interactions. Balancing E2E and other testing approaches is pivotal for efficient and effective test automation strategies.
