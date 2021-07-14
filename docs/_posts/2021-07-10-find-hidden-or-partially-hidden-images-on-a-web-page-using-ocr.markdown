---
layout: post
title: "Find hidden or partially hidden images on a web page using image recognition technology"
date:   2021-07-10 14:40:30 -0400
categories: web testing 
tags: WCAG java selenium opencv
---
![](/images/hidden_images.jpg)

Danny Briskin, QA Consultants Senior Automation Engineer
 
# The issue
The process of design and creation of a web page usually starts with creation of a wireframe that includes images. It is obvious that a web designers wants to see those images on the page in a way everything was originally designed.
But sooner or later, some changes are to be made to the page. Business require new interactive elements, new stylesheets, new version of 3rd-party libraries. All that could lead to changes that sometimes cannot be seen by a human tester or automated scripts.
I'm talking about images that are placed on the page but after a change they partially or completely disappear from viewport.
[before](before.jpg)
[after](after.jpg)

How to automatically determine that an image that has to be shown by design is really visible and not hidden by some other design element?

# Approach
We can automate web pages browsing using Selenium web driver. During visiting the page we can find all images links in the web page source and download all of them in the original quality.
The next step is to emulate a user's viewport. Sure, we can make a screenshot of an entire page but the quality of each image on the big picture could be low and, moreover, the page could have very large height (i.e., you have to scroll and scroll down, infinitely).
A better approach is to make a screenshot of each place where an image is supposed to be (according to HTML source).
The last thing we need to do is to compare two images: original and the screenshot.
There are several approaches about images comparison, some of them have high precision but needed a lot of resources, some have drawbacks not applicable to this specific task.
We have chosen a [comparison](https://docs.opencv.org/3.4/d8/dc8/tutorial_histogram_comparison.html) based on [image histogram](https://en.wikipedia.org/wiki/Image_histogram).
The precision is not the best but it is quite adequate for the most of web pages.

# Implementation 
Java + Selenium was chosen as a framework basis and [OpenCv](https://opencv.org/) was chosen asa image recognition library.
There are several steps of comparison:
- Check that both images have the same dimensions. This could be a web performance issue when dimensions ratio is high but for our goal we just equalize dimensions.
- Convert both images to greyscale. It is possible to compare histograms of colored images but it adds some complexity and fires a lot of false positives in results.
- Create histograms and [normalize](https://en.wikipedia.org/wiki/Normalization_%28image_processing%29) them
- Calculate the distance between two histograms (a number from 0 to 1)

If the distance is close to 0 it means that images are almost identical. The value that is close to 1 means that user cannot clearly see the image on the page.

# Summary
Significant difference between human and automatic testing is in the fact that the script cannot 100% emulate an user. In the case above for example, script can be absolutely sure that image is present and visible on the page but in fact it is not. Now, there are a lot test types that cannot be done by automation software but one can strive to make that number less.
