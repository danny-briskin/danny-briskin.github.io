---
layout: post
title: "Setup AspectJ"
date:   2020-08-01 15:40:30 -0400
categories: java
tags: java lombok AOP AspectJ
---
![](/images/AOP.webp)
Danny Briskin, QA Consultants Senior Automation Engineer
August 2020

# Common part
Download AspectJ installation stable release. Version 1.9.5 is used everywhere in the article. Install it to 
```{your-path-to-aspectj}```.

# Intellij Idea Ultimate edition
If you have an ultimate version of this popular IDE, the setup is very simple.

Open settings:
```
File | Settings | Build, Execution, Deployment | Compiler | Java Compiler
```
You need to choose ajc compiler and specify the path to the installed Aspectj library
```{your-path-to-aspectj}/lib/aspectjtools.jar```
![Settings](/images/ajc-01.webp)
{% highlight java %}
{% endhighlight %}
