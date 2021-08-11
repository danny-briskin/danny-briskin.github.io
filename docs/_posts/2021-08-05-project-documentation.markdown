---
layout: post
title: "The role of project documentation"
date:   2021-08-05 14:40:30 -0400
categories: documentation 
tags: documentation
---
![](/images/project_documentation.jpg)

Danny Briskin, QA Consultants Senior Automation Engineer


# The issue
In any project, whether it is a software development or testing or support, the importance of adequate and up-to-date documentation should not be underestimated. If a project relies only on "sacred knowledge" of its personnel it is doomed. If the documentation exists but is not up-to-date it will lead to imminent errors in development, testing or support routines. If documentation is up-to-date but lacks of details and links to other documents it will force project personnel to spend longer time to figure out with obvious issues, and that can increase total project budget. If documentation exists and regularly updated but has questionable consistency and reliability (i.e. there are several points of truth) it will lead to useless work done with false basis and possible conflicts in the team.

# What is to be documented
The first precondition is: there must be documentation in the project. Don't join a project without it. Well, if you ought to, do it yourself. Be 100% sure it will pay you and your team out very soon.
A plain text file, saved on your local PC with some portion of documentation is better than nothing.
## Artifacts 
Whatever your project is working on, it should have a document. Whether it is database table, class or method in the code, a server to maintain, a utility to run - everything. Use built-in capabilities of collaboration system you use, or create your own documents. Use cross links to other documents to tie up the documentation and navigate through it. 
## Processes 
Create a step-by-step instruction for all processes manual execution routines. Create description for automatic processes (inline it in scripts if possible) to be able to repeat it manually.
Don't hesitate to include screenshots, even for obvious operations, sometimes those instructions are executed in a stress environment by not highly trained personnel.
## Environments
Write out all variable values related to each environment you use to be able to recreate it if needed. Collect all scripts needed to be run in the environment, prior to start usage.
## Thoughts
Create a blog. Be able to record down you thoughts about artifacts, processes, etc. Put there any ideas you want to implement in the future. Those little documents can be easily transformed into user stories and tasks.


# How to document
Don't assign a single person to be responsible for documentation creation. Encourage all team members to take part, it will not only increase involvement but can strengthen team knowledge and boost information exchange between colleagues.
Add an option to vote for the best documents, people should be recognized for their efforts.

Introduce a versioning system into your documentation project. It is always useful to return to one of previous versions of an instruction and compare it with current one. Beside the history, you can always track changes by author. 

Discussion in an important part of human culture, use it wisely in documentation. Add an option to comment all documents (and comments as well) but be careful, don't turn comments into more valuable thing than the document itself.

Take care of sensible information. Be able to share documentation with those who have appropriate access and hide it from others. Make sure that relevant stakeholders have access to the documentation they needed.

It is a good idea to have a technical writer role in the project. Not all specialists are masters of words, and taking in account language and cultural diversity in contemporary projects, the documentation they produce could be hardly readable. Technical writer can polish the documentation and make its appearance standardized. 

# Tools
- A free [MediaWiki](https://www.mediawiki.org/wiki/MediaWiki) tool or any standalone or hosted Wiki-like tools. With the all power of Wikipedia, you can create and maintain a highly customizable documents. There are comments and discussion, a lot of plugins. Unfortunately version control and user management are very primitive. The main drawback of the system is in lack of native links to other collaboration tools (like task management, version control system, etc.)
- [Atlassian] (https://www.atlassian.com/). Well-known Atlassian tools for IT projects like Jira and Bitbucket. There is also a dedicated tool: Confluence but even with a very basic Wiki, coming with Bitbucket repository, you can achive great results in documentation.
- [Mictrosoft Azure](https://azure.microsoft.com/en-ca/) a paid tool from Microsoft with all its excellence.
- [GitHub] (https://github.com/) a built-in documnetation system in Git repository is not very powerful but is very handy (yes, this article was created in that tool)
- [Gitlab] (https://about.gitlab.com/) - another version control system with documentation tools onboard

# Summary
Create and review and maintain your documentation every time! "Boring and useless" work will can turn your project from disaster to success. You team members will be grateful for your efforts and even if you leave the project, future generations will be excited to have such a farsighted predecessor.