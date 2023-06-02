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
In software development projects, it is customary to establish various distinct environments to cater to different objectives. Typically, these include a designated development environment (DEV), a dedicated testing environment (QA), and naturally, a production environment (PROD). In more intricate scenarios, additional configurations such as User Acceptance Testing (UAT) and System Integration Testing (SIT) environments are incorporated. Moreover, in cases where a significant volume of testing activities is involved, it is common to have multiple instances of each environment type

In a normal deployment workflow, new versions of software are deployed in the order:
1. DEV
2. SIT
3. QA
4. UAT
5. PROD

Consequently, the PROD environment may have version 1.3 in PROD, while UAT has 1.4, QA - 1.5, SIT - 1.6 and DEV 2.0. Although the version numbers may vary, and there can be more significant gaps between them, the underlying concept remains consistent. Once testing is conducted in a given environment, its respective version is promoted to the subsequent environment, replacing the current version with a "ready-to-deploy" update from the lower environment.

However, certain challenges can arise within this scheme. Testers, including end users, may identify defects within any environment, prompting developers to swiftly generate a hotfix that should be tested as well. Nonetheless, difficulties emerge when ongoing testing is underway in an environment that cannot be interrupted to facilitate the deployment of an updated version. Consequently, the fixed version will need to be deployed to another environment. If there is a scarcity of available environments, a chaotic situation can ensue.

For instance: Suppose a defect is found in the PROD (ver. 1.3), prompting the development team to create version 1.3.1 with the necessary bug fix. But there is an ongoing testing of ver. 1.4 in SIT, so let's deploy it in QA. Meanwhile, another defect was found in UAT (ver. 1.4), leading the developers to produce version 1.4.1 (that does not include bugfix from 1.3.). The updated version is then deployed back to the UAT environment. At this point, the QA team completes their testing and prepares to deploy version 1.3.1 to UAT, but encounters the ongoing testing of version 1.4.1. Consequently, the SIT environment becomes available, and the decision is made to deploy version 1.3.1 there temporarily, mimicking a UAT environment.

Now we have:
1. DEV (ver. 2.0)
2. SIT (ver. 1.4.1)
3. QA (ver. 1.3.1)
4. UAT (ver. 1.4)
5. PROD (ver. 1.3)

# Wearing a test automation hat
Let's consider the scenario where you possess a test automation framework with a comprehensive regression suite, prepared for deployment at any given moment. Being a proficient software developer [in test], you are well-versed in utilizing version control systems. Naturally, you maintain a master (main) branch, housing the most up-to-date codebase. When making modifications to the framework, you adhere to version control guidelines by creating a branch, committing changes, pushing them to the repository, and ultimately generating a Pull Request to merge the changes into the master branch.
Well done!

Assuming that you have made the necessary adjustments to your tests to align with the new bug fixes introduced in versions 1.3.1 and 1.4.1, you are now prepared to execute the regression suite. Typically, the regression suite is executed against the QA environment (1.3.1). Therefore, let's clone the latest version from the master branch and execute it. However, a predicament arises concerning the changes made for versions 1.4.1, as they have not been incorporated yet. Consequently, a temporary solution entails temporarily removing those specific changes from our automation framework. Voila! It works as expected!

Nevertheless, we now encounter a situation where testing needs to be conducted on a version deployed in the SIT environment (1.4.1). Consequently, we must reintegrate those previously removed changes back into our framework. Naturally, if we aim to uphold version control practices, all these application and rollback processes should be executed through the Branch-Commit-Push-PullRequest sequence.

Consider the complexity that may arise when dealing with additional environments or larger version gaps, as the number of changes between versions increases.

# How to ease this pain
Let's implement a solution that involves maintaining multiple master branches within the repository. While technically only one master branch can exist, we can adapt our branching policy to accommodate this approach. We can establish an agreement where the initial master branch solely contains the latest code. For each version of the system under test, particularly major versions, we will create corresponding branches named "branch-X.Y.Z," where X.Y.Z represents the version number.

Whenever a change is made for a specific version, such as 1.3.1, that is not the latest version (assuming the latest version in the given example is 2.0.0), we will push that change to the following branches:
1. The "branch-1.3.1" branch.
2. All higher version branches where the change is currently applicable or will be applicable in the future. However, the decision to apply the change to the "branch-1.4.1" branch, for instance, will depend on specific considerations. Nevertheless, the change will be applied to the "branch-1.5," "branch-2.0," and "master" branches.

Implementing the second aspect of this approach is complex and necessitates comprehensive knowledge of the system under test, the automation framework, and the project's processes. However, investing time and effort into implementing this solution is highly beneficial.

By adopting this approach, you will have the flexibility to launch any regression suite against any version of the system under test in any environment, either individually or simultaneously across multiple versions.

# Summary
Implementing and maintaining multiple master branches in version control can be a challenging undertaking. It requires significant time and effort, and may initially appear impractical. Therefore, I do not recommend adopting this approach right from the outset. It is crucial to first understand the types and quantity of environments involved, determine which environments are necessary for running automation tests, comprehend the versioning structure in the project, and become familiar with the existing deployment guidelines and workflows. Only if the landscape proves to be exceptionally complex should you consider transitioning to the recommended multi master-branch scheme. Otherwise, it is advisable to keep the version control approach simple and straightforward.
