---
layout: post
title: "Testing web pages with Shadow DOM"
date:   2023-04-26 14:40:30 -0400
categories: web testing 
tags: xpath, testing, web ui, shadow dom
---
![](/images/performance_testing.jpg)
Danny Briskin, Quality Engineering Practice Manager


# The issue
According to [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_shadow_DOM), Shadow DOM allows hidden DOM trees to be attached to elements in the regular DOM tree - this shadow DOM tree starts with a shadow root, underneath which you can attach any element, in the same way as the normal DOM.
It looks like a contemporary implementation of old-time frames. The only difference is that frames HTML code is a fully qualified HTML, with all required html tags, while shadow DOM can start with literally anything.
What does it mean for QA automation engineers? 
First, [XPath](https://www.w3schools.com/xml/xpath_syntax.asp) stops working in developer tools section of your browser. It definitely adds a headache when you are trying to find an element you need. What's more, there can be no visible connection between elements. For example, you can find a combo box that is implemented with "div" elements like:
{% highlight html %}
<div class="select-wrapper" id="my-shadow-root"></div>
{% endhighlight %}
![](/images/changelog_02.png)

There are no items in it but when you click on it, some items are populated. Sure, there was a JavaScript and CSS code that produced options in runtime. Some developers are skilled enough and make it appears inside a pseudo-select tag (i.e. top level div), like this:
{% highlight html %}
<div class="select-wrapper" id="my-shadow-root">
  #shadow-root  
      <div class="select">
        <div class="select__trigger">
          <span>Option 1</span>
          <div class="arrow"></div>
        </div>
        <div class="custom-options">
          <span class="custom-option selected">Option 1</span>
          <span class="custom-option">Option 2</span>
          <span class="custom-option">Option 3</span>
        </div>
      </div>
    </div>
{% endhighlight %}
![](/images/changelog_03.png)
You can see it visually, but it's not easy to search for it programmatically. 

# XPath fails
Let's try to search for it using XPath:
{% highlight  %}
//span[@class='custom-option']
{% endhighlight %}    
Oops, nothing... 
![](/images/changelog_04.png)
But it is there! What a pity...
![](/images/changelog_01.png)

# Selenium can do it!
Well, pure JavaScript can do it too, and Selenium uses those methods to achieve the same goal.
Let's first replicate that unsuccessful search with Selenium:
{% highlight java %}
webDriver.findElement(By.xpath("//span[@class='custom-option']"))
{% endhighlight %} 
The same result, no wonder
![](/images/changelog_05.png)

Let's locate the div that contains that shadow-root element:
{% highlight java %}
webDriver.findElement(By.id("my-shadow-root"))
{% endhighlight %} 
At least that one is found.
![](/images/changelog_06.png)

There is a method, called "getShadowRoot()" that can give you a root element of the inner structure:
{% highlight java %}
webDriver.findElement(By.id("my-shadow-root"))).getShadowRoot()
{% endhighlight %} 

Now, we can continue our search as we are "inside" a regular WebElement.
{% highlight java %}
webDriver.findElement(By.id("my-shadow-root")).getShadowRoot().findElements(By.className("custom-option"))
{% endhighlight %} 
![](/images/changelog_07.png)

Pay attention, that not all "By.*" methods work fine inside a shadow-root. For the moment (Apr'2023), only by class and by id are working.

# A worst-case scenario
There are a lot of modern-day frameworks like Angular and ReactJS, that require a few developers' work and can do a lot of magic by itself. So, it is quite possible, if you collaborate with a regular developer with a powerful framework in hands, you will see still empty combo box and this code somewhere in the DOM:

{% highlight html %}
<div class="select-wrapper" id="my-shadow-root"></div>

........

<div class="zxc123po">
  #shadow-root  
      <div class="select">
        <div class="select__trigger">
          <span>Option 1</span>
          <div class="arrow"></div>
        </div>
        <div class="custom-options">
          <span class="custom-option selected">Option 1</span>
          <span class="custom-option">Option 2</span>
          <span class="custom-option">Option 3</span>
        </div>
      </div>
    </div>
{% endhighlight %}    
How to link those options with your combo box element (keeping in mind that class name is a random, obfuscated string)? The answer is: "Sorry, but no way". 
The only way is to explain to the developer what Software Testability is, and ask them to change their code in order to have a reliable way of identifiying elements.

# Summary
I believe that sometime, all browsers will have a support of XPath looking inside shadow DOMs and Selenium will introduce a more natural way of working with those elements.
