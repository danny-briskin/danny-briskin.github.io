---
layout: post
title: "Perspectives of Automation Engineers: The Perils of Boilerplate code, even in test automation"
date:   2021-07-07 13:40:30 -0400
categories: java lombok
---
Danny Briskin, QA Consultants Senior Automation Engineer
July 2020
{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}


Today, QA has become more and more reliant on automation. Sure, it saves time and increases coverage, but it also requires specific knowledge within test automation approaches, languages, and tools. Programming languages like Java are the most common choice to create test automation frameworks today. However, as automation frameworks and their test suites grow, more and more lines of code are required, and it may become cumbersome to maintain.  Without a focus on reducing code complexity and efficiency, automated test developers face the risk of perpetuating the same issues we find in code under test, but in our test automation code.   

Let’s discuss ways to decrease the number of lines without losing application functionality.  

This blog assumes the reader has a working knowledge and experience in java programming. 

When it comes to coding, some programming languages like C, C++, and Java require every little detail be fussed over, scrutinized, and reviewed with a fine-tooth comb. Learning to code with these intricate nuances becomes a craft in itself. This may require a coder to write a lot of sections of code during software development that becomes computer programming boilerplate code.  

According to computer science, the syntax of coding is a language the computer can understand and a set of rules that defines the combinations of symbols which are considered to be a correctly structured document or fragment in that language. 

Once you’ve mastered computer coding, and come to realize the headaches that come with it, you have to spend a lot of time creating absolutely obvious, repetitive, similar lines of code just because it is required. The process is painful even if you use helper functions of your IDE. 

There are other languages like Python and JavaScript that allow you to code less, removing that burden. Once you’ve evaluated and reviewed the code and everything is satisfactory, you can concentrate on programming business logic mode. 

Except, one never really knows what’s going on under the hood. It is quite possible that built-in routines work efficiently, but maybe they don’t? Moreover, when your application grows, and you would like to make it more flexible, language limitations limit that fluidity. 
