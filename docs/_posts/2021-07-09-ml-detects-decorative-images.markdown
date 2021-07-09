# Motivation
During accessibility testing ([WCAG](https://www.w3.org/WAI/standards-guidelines/wcag/)) of a website one of the most confusing tests is about decorative images. The requirement says:
Text Alternatives
: **Provide text alternatives** for any non-text content so that it can be changed into other forms people need, such as large print, braille, speech, symbols or simpler language.
but on the other hand
**Decoration**, Formatting, Invisible
: If non-text content is pure decoration, is used only for visual formatting, or is not presented to users, then it is implemented in a way that **it can be ignored** by assistive technology.

In other words, if an image is informative one it should have **alt** attribute filled with intelligible description. Otherwise, alt attribute must be empty.

# The issue
Well, it is obvious for a webpage author which image is decorative. It is not complicated for a human tester to do the same. But how to determine it automatically?
There are no reliable approaches to solve this issue based on size or color of images because of the nature of webdesign.
All decisions about image decorativeness can be done based on contents of the image. But it is not the only meaningful criteria to check.
The same images could be decorative on certain pages while on others it will not. 
A picture of a pretty women in underwear will be quite informative in women underwear shop catalog but it becomes purely decorative in online banking website. 

Let's summarize: for an image to be informative, its topic should be the same as its page contents topic.

# Idea
We are faced with a classic [Classification Task](https://en.wikipedia.org/wiki/Statistical_classification). An more precisely, there are 2 classifications: one for page text and other one is for image. If classes are the same, the image is marked as informative, otherwise - decorative.
Nowadays, machine learning technologies allow to make reliable decisions in a relatively short time.
The simplest way is to use Python as a programming language and one of well-known libraries to create two classifiers.
There are some drawbacks in that solution:
1. we need to create a list of all possible topics
2. and we need to train a model on a lot of examples to achieve good accuracy

# Attempt number one
I started with text classifier(yes, it looked much simpler than the one for images). Using [spacy](https://spacy.io/) library and Wikipedia (English) slice of 2017, [Google News](https://news.google.com/) and several RSS news feeds as a training examples I trained a model with 91% of accuracy in predefined 19 categories.
The number of training examples was not big enough, I was working on increasing it, but the 1st drawback worried me much more. What if text doe not relate to any of given topics? Is it a good idea to increase the number of topic and how it will affect to accuracy?
Thinking of that I gradually stopped the development.

# Attempt number two
Once I have read about [Zero shot learning](https://en.wikipedia.org/wiki/Zero-shot_learning) and [CLIP](https://openai.com/blog/clip/). CLIP stands for Contrastive Language-Image Pre-training. "It can't be possible!" was my first thought. How it work with **any** image and **any** text? Literally, any! There are models in that library but one cannot train a model for all images in the world. A kind of magic...

I started to verify it with some simple tests:
1. ![A cow](cow.jpg)
|a boy|0.0001 %|
|a cow|99.98 %|
|a horse|0.01 %|
|BMW|0.001 %|
|a cowboy is riding through prairie|0.01 %|

Well, it was simple. Let's make it a bit confusing (images were taken from a web shop catalog):

2. ![what it is?](1.jpg)
What do you think it is? A candy? A toy? No, it is "Candy Corn Dancin Tricky Treat Singing Stuffed Animal With Motion 10" - 98% of confidence

and this one?
![sweater](2.jpg)
"CHAMPION HERITAGE OVER SHOULDER SCRIPT L/S – MEN'S" - 99%

and finally, here it was not absolutely sure
![A cow](3.jpg)
"Aeropostale Marilyn Monroe Graphic Tee - Navy, Medium" - 62%

Well, okay, even me, a human, is not sure what do they sale using this picture (the answer is - a T-short)

So, it is working. Let's make it usable.


# Architecture
Let's assume that we have a WCAG testing software and we will provide it a service that receives a link to a picture and several (>=1) texts/words/sentences that are possible topics of given image.
The response is the probability for each given text that the topic of the image is the same.

The request:
```
{ "image_url": "https://somesite.com/image.jpg",
    "image_texts": [
        "Mens Naruto Hero Of The Hidden Leaf Tee - White",
        "Candy Corn Dancin Tricky Treat Singing Stuffed Animal With Motion 10",
        "Old Skool Rainbow Checkerboard    - Little Kid - Black / Multi",
        "Converse Chuck Taylor All Star Lo Sneaker - Little Kid - Black",
        "DISNEY MICKEY MOUSE QUILTED OH BOY CROSSBODY",
        "Christmas Lady & Tramp",
        "Aeropostale Marilyn Monroe Graphic Tee - Navy, Medium",
        "CHAMPION HERITAGE OVER SHOULDER SCRIPT L/S - MEN'S"
    ]
}
```

The response:
```
[
    {
        "probability": 0.0010680541163310409,
        "text": "Mens Naruto Hero Of The Hidden Leaf Tee - White"
    },
    {
        "probability": 2.7145318881593994e-07,
        "text": "Candy Corn Dancin Tr <...> nimal With Motion 10"
    },
    {
        "probability": 0.01749918796122074,
        "text": "  Old Skool Rainbow  <...>  Kid - Black / Multi"
    },
    {
        "probability": 5.644326392939547e-06,
        "text": "Converse Chuck Taylo <...> - Little Kid - Black"
    },
    {
        "probability": 0.00018363054550718516,
        "text": "DISNEY MICKEY MOUSE QUILTED OH BOY CROSSBODY"
    },
    {
        "probability": 0.00025380559964105487,
        "text": "Christmas Lady & Tramp"
    },
    {
        "probability": 4.111427188036032e-05,
        "text": "Aeropostale Marilyn  <...> c Tee - Navy, Medium"
    },
    {
        "probability": 0.9796881079673767,
        "text": "CHAMPION HERITAGE OVER SHOULDER SCRIPT L/S - MEN'S"
    }
]
```

Swagger-like UI should be created to ease service usage. 
The service should be easily distributed to any kind of environment and does not depend on environment.

# Implementation
[Flask](https://flask.palletsprojects.com/en/2.0.x/)/[Waitress](https://docs.pylonsproject.org/projects/waitress/en/stable/) was chosen as service engine. [Swagger-UI](https://pypi.org/project/swagger-ui-py/) will serve Swagger documentation.
Everything will be packed into [Docker](https://www.docker.com/) container.

