---
layout: post
title: "Test Automation in multiple versions and environments"
date:   2023-06-02 14:40:30 -0400
categories: testing 
tags: testing, git, branches, environments, versions
---
![](/images/rest_hypermedia.jpg)
Danny Briskin, Quality Engineering Practice Manager


# The issue
Usually, in software projects there are multiple environments set up for different purposes. At least, there is one is for development (DEV), one for testing (QA) and there is a production environment (PROD) of course.
More complex configurations include UAT (User Acceptance Testing) and SIT (System Integration Testing) environments. And, if there are a lot of testing activities to be done, there can be several environments of each kind.
In the normal deployment workflow, new versions of software are deployed in the order:
1. DEV
2. SIT
3. QA
4. UAT
5. PROD

So, one can see version 1.3 in PROD, while UAT has 1.4, QA - 1.5, SIT - 1.6 and DEV 2.0. Numbers may differ, there can be larger gaps between versions, but I guess you've got the point. Whenever testing is done in an environment, its version is deployed to a higher environment and is replaced by a "ready-to-deploy" version from a lower one.

What can go wrong in this scheme? Testers (including end users) can find a defect in any environment. And developers will quickly produce a hotfix that should be tested as well. But what if there is ongoing testing in a certain environment that cannot be stopped (to deploy an updated version)? Right, that fixed version will be deployed elsewhere, to another environment. And if we have a lack of them, a mess will start!

For example: There is a defect in PROD (ver. 1.3), developers team produces ver. 1.3.1 (with fixed defect) but in SIT, there is a testing of 1.4, so let's deploy it in QA. Okay, meanwhile, in UAT (ver. 1.4) was found another defect, so developers have produced 1.4.1 (that does not include bugfix from 1.3.) and deployed it, where? Ah, okay, to UAT. Now, QA team has finished their testing and ready to push 1.3.1 to UAT, but there is a 1.4.1 testing... Oh my... SIT is free - let's deploy it there to pretend that is a temporary UAT.

Now we have:
1. DEV (ver. 2.0)
2. SIT (ver. 1.4.1)
3. QA (ver. 1.3.1)
4. UAT (ver. 1.4)
5. PROD (ver. 1.3)

# Wearing a test automation hat
Suppose you already have a test automation framework with a regression suite, ready to launch at any time. You are a decent software developer [in test] and you do know how to work with version control systems. Sure thing you have a master(main) branch where up-to-date code is situated. When you do changes to the framework you follow version control guidelines, you create a branch, make commits, push your changes into repository and. finally, create a Pull Request to merge changes into the master branch.
Good job!
Assuming, you have changed your tests to be compliant with new bugfixes from versions 1.3.1 and 1.4.1 and are ready to run regression suite.
Normally, your regression suite is run against QA environment (1.3.1), so let's clone the latest master branch version and run it. But wait, there will be an issue with those 1.4.1 changes, they are not yet there. Okay, let's temporarily remove those changes from our automation framework. Here we go, it works!

But here we have a situation when we need to test a version that was deployed elsewhere. We need to run it in SIT (1.4.1). So, we need to apply those changes back. 
And if you are going to follow version control rules, all those apply/rollback should be done via Branch-Commit-Push-PullRequest chain.
What if there a more environments? What if the gap (the number of changes) between versions is even bigger?

# How to ease this pain
Let's maintain multiple master branches in the repository. Yes, technically there can be only one, but no one can restrict to apply your rules to branching policy. Let's agree that the initial master branch is to contain only the latest code. For each version of the system under test (major version, at least) let's create a branch, called "branch-X.Y.Z" (where X.Y.Z is a branch number). Whenever we have a change for a specific version (say, for 1.3.1) that is not the latest one (the latest in the example above is 2.0.0) we will push the change to:
1. "branch-1.3.1" branch
2. All higher versions branches where that change is (or will be) applicable. Yes, we need to decide whether to apply it to "branch-1.4.1", it depends. But we will apply it to "branch-1.5", "branch-2.0", and "master" branches.

The second item is complicated and requires a lot of knowledge of the system under test, automation framework and processes in the project. But it is worth the time and effort spent!
At any point of time, you will be ready to launch any regression suite against any version of system under test in any environment, or in all of them in parallel, all versions!

# Summary
Maintenance of multi master-branches in version control is not an easy task. It consumes a lot of time and effort and looks ridiculous at first sight. I don't recommend using it right from the start. You must find out what type and number of environments there are. Which of them you will need to use to run automation tests? How versioning is set up in the project and what deployment guidelines and flows exist. Only if the landscape is that complicated, try to switch to the recommended multi master-branch scheme. Otherwise, keep it simple!
