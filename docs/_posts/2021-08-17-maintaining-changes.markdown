---
layout: post
title: "Maintenance of system changes and changelog automation"
date:   2021-08-17 14:40:30 -0400
categories: documentation 
tags: changelog
---
![](/images/changelog.jpg)

Danny Briskin, QA Consultants Senior Automation Engineer


# The issue
In a project with a long history, it's often good to have all information about changes is stored. Not only the date, author and content of the change but also a motivation that led to the change.
Using a version control system(VCS) can greatly help in backtracking but its capabilities are very scarce.
In addition, VCS contributors are usually concentrated on contributing rather than description of what was done. What's more, work on the project can be done in parallel, by several people, not in a "logical" sequence (from business perspective). For example, a customer login form could be implemented much earlier than customer registration form.
And if you have to maintain several versions in the same time, questions like "What was changed in version X?" and "Why that change was (or was not) included in version Y?" can become a nightmare.
In that case, a proper tracking of changes could be vital in project.

# Changelog
A changelog is a file, where changes that were made since previous version, are described. A typical chagelog looks like
{% highlight markdown %}

## 1.2.0 (2021-08-04) Feature

**registration** new registration form

## 1.1.1 (2021-08-03) Bug Fixes

 **login** correct minor typos in code
 
## 1.1.0 (2021-08-03) Feature

**login** login form was added

## 1.0.0 (2021-08-01) Initial release

{% endhighlight %}
In other words, a Changelog is a composition of Release Notes (or you can pick a release not from your Changelog).


That's not bad in general (and it is much better than you don't have any changelog!). But it begs questions: "How this text document is related to VCS or task management system? How to find contents of the change mentioned in version 1.1.0?" 
A support engineer has to spend a lot of time (with not guaranteed result!) to search repository and for that specific change. And when/if it is found, the commit message could be even more scarce, like 
```
fix bug
```

When project grows bigger, it takes more and more resources to maintain the changelog. And yes, this is a manual work, needed to be done by someone.


# Git log becomes a changelog! And failed...
Well, as you know, those developers ought to write some commit messages anyway. Let's not make the same work twice! Can we convert a set of messages into a changelog? Easily, with the power of "git log" command:

{% highlight bash %}
# like this
git log --pretty="- %s"
# or like this
git log  --pretty=format:'<li><a href="http://github.com/danny-briskin/classifyIt/commit/%H">view commit &bull;</a> %s</li> ' --reverse 
# even between versions (tags) if you have it in the repo
git log v2.0.0...v3.1.0 --pretty="- %s"
{% endhighlight %}

Each of those command will bring us a file that looks like a changelog. At first sight.
It will consists of first lines of commit messages (yes, we can play with --pretty and --format parameters to get the full message), dates, authors - things we need. What can go wrong?
Those messages... Have you seen it? 
- Bug fix
- Code was fixed
- created variable instead of parameter
- new attempt for the latest Jira task
- ...
Will it help us in our investigations? Of course not.

What if we forgot to tag a commit to mark a specific version? How can we determine now, what was changed since version 1.0.0? Between 1.1.2 and 1.2.5? And why it was changed?

# Let's enforce proper commit messages
We can use [Git hooks](https://git-scm.com/docs/githooks) to prevent contributors making commits, violating some rules we want to see in commit message.
For example, if we change commit-msg hook to something like

{% highlight python %}
#!/usr/bin/env python3
 
import sys
import re
 
with open(sys.argv[1]) as file:
    content = file.read()
    if (not re.match(r'\[JIRA-\d+\] - .+', content)):
        raise NameError("Please add Jira task number in commit message! {}".format(content))
{% endhighlight %}

it will prevent user to commit without a "link" to the task that commit is related to. I.e.
{% highlight bash %}
git commit -m"Test" MyFile.java
{% endhighlight %}
will fail, while
{% highlight bash %}
git commit -m"[JIRA-123] - Test" MyFile.java
{% endhighlight %} 
will succeed.

In the same way, we can enforce users to add a type of change (from predefined list), business area and other useful information.
In fact, all specification exists and can be used. See [Semantic Version](https://semver.org/) or [Conventional Commits](https://www.conventionalcommits.org/).

There are only two drawbacks in git hooks:
1. Hooks are not saved in version control, so it is not easy to propagate it to all project participants.
2. Hooks are not always crossplatformed. You need to use a high level programming language to make it work crossplatform but you have to have that language installed on every device where hooks are used.

On the other hand, there are systems with built-in (and highly customizable) hooks, like Bitbucket.
A "must have" thing to turn on there is [Require issue keys in commit messages](https://support.atlassian.com/bitbucket-cloud/docs/link-to-a-web-service/) and you can enjoy commits with a link to task number.

# Let's automate changelog
Having forced contributors to make intelligible commit messages, let's automate changelog creation.
Let me introduce one of such applications: [Standard Version](https://github.com/conventional-changelog/standard-version). It uses [Node.js](https://nodejs.org/en/) as an engine, so it has to be installed prior to use.
Even though it was designed for [Angular](https://angular.io/) it can be easily used in most of software project that have Git as a VCS.
The idea of the application is to maintain previous version (that was already in chahgelog) and add all new commits (done in a Conventional way) as a release notes between previuous and current versions. It makes tag for the new version in Git, and changes previous version to new one in configuration file(s) for the project.
The application is customizable, using command line parameters and configuration files you can achieve a perfect changelog.
<details>
  <summary>An example of generated Changelog</summary>

{% highlight markdown %}
My Application changelog 

## [4.0.0](https://github.com/danny-briskin/aopArticle/compare/v3.1.3...v4.0.0) (2021-08-18)


### ⚠ BREAKING CHANGES

* **login-form:** Login and registration form are ready for production ([23b1515](https://github.com/danny-briskin/aopArticle/commit/23b150034de5e32faca0398787be8e5ba88ca89c)), closes [#5](https://github.com/danny-briskin/aopArticle/issues/5)


### [3.1.2](ttps://github.com/danny-briskin/aopArticle/compare/v3.1.1...v3.1.2) (2021-08-18)


### Bug Fixes

* **login-form:** URL was changed to PROD ([23b1513](https://github.com/danny-briskin/aopArticle/commit/23b150034de5e32faca0398787be8e5ba88ca89c)), closes [#13](https://github.com/danny-briskin/aopArticle/issues/13)

### [3.1.1](https://github.com/danny-briskin/aopArticle/compare/v3.1.0...v3.1.1) (2021-08-18)


### Bug Fixes

* **login-form:** correct minor typos in code ([23b1512](https://github.com/danny-briskin/aopArticle/commit/23b150034de5e32faca0398787be8e5ba88ca89c)), closes [#12](https://github.com/danny-briskin/aopArticle/issues/12)

## [3.1.0](https://github.com/danny-briskin/aopArticle/compare/v3.0.0...v3.1.0) (2021-08-18)


### Features

* **login-form:** A new feature for Login form ([23b1503](https://github.com/danny-briskin/aopArticle/commit/23b150034de5e32faca0398787be8e5ba88ca89c)), closes [#3](https://github.com/danny-briskin/aopArticle/issues/3)

## [3.0.0](https://github.com/danny-briskin/aopArticle/compare/v2.0.0...v3.0.0) (2021-08-18)


### ⚠ BREAKING CHANGES

* **registration-form:** Registration form captcha was added ([23b1501](https://github.com/danny-briskin/aopArticle/commit/23b150034de5e32faca0398787be8e5ba88ca89c)), closes [#2](https://github.com/danny-briskin/aopArticle/issues/2)

## [2.0.0](https://github.com/danny-briskin/aopArticle/compare/v1.0.13...v2.0.0) (2021-08-18)


### ⚠ BREAKING CHANGES

* **registration-form:** Registration form is working ([23b1500](https://github.com/danny-briskin/aopArticle/commit/23b150034de5e32faca0398787be8e5ba88ca89c)), closes [#1](https://github.com/danny-briskin/aopArticle/issues/1)


## 1.0.0 (2021-08-17)

{% endhighlight %}
</details>

# Summary
Having a proper Changelog can greatly boost your system maintainability. It could ease not only backtracking but enables a clear view of system development through time, and links together version control, task management and support procedures.
