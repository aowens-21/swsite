---
title: "Doing the Django Tutorial"
date: 2018-01-29
draft: false
---

## Doing the Django Tutorial
Code is available [here](https://github.com/aowens-21/intro-django).

This is going to be a pretty simple and short blog post, as I didn't get to do much in the way of interesting new things this week. However, I did
check out the [Django Framework](https://www.djangoproject.com/) for Python, and I really enjoyed making a small web application in that. I followed the
[Official Tutorial](https://docs.djangoproject.com/en/2.0/intro/tutorial01/) and I think that's a great intro to the framework in general.

I have a lot of ideas for small apps to make in the future, and I look forward to making more for future blog posts. For now I guess I'll talk
about some of the features I liked in Django.

### Structure of Projects
What I really mean by this is the idea in Django that every project can be made of many different "apps", which makes code reuse in many
different projects very easy. The way you structure your code is important in any decent sized project, and while I can't speak to Django's scalability
from personal experience, it definitely seems to have taken that factor into consideration.

For example, in the Polling project that the tutorial has you making, you have one project: polls. This codebase is all you need to make something so small, but
the potential to reuse some or all of the code from that app in a totally different Django project is powerful.

### Testing features
Django's tutorial [dedicates a whole section](https://docs.djangoproject.com/en/2.0/intro/tutorial05/) to testing, and as someone who
is interested in [Test-Driven Development](https://en.wikipedia.org/wiki/Test-driven_development), that is a huge plus.

Django features extensive testing tools for your models and for how your views are populated, and the way this is set up is in your application's `tests.py` file.
This structure can be changed and is very flexible, but by default Django searches for any functions that start with "test", so you can sort of design your test
structure however you like. Another thing to mention is that testing configurations work out of the box and you don't have to do really anything as far as I can tell
to start testing immediately.

### Ease Of Use
This is a big one, because a lot goes into a modern web application. Django sets up so many things including routing, views, and database integration for you,
so you can focus more on writing the actual logic for your application instead of boilerplate setup. That's not to say that boilerplate isn't useful or necessary
in Django, as I'm sure that larger and more complex projects probably have to get their hands dirty with deeper configuration and setup.

The fact is though that while I might end up trying out something lower-level like [Flask](http://flask.pocoo.org/), which is also very promising, I'm really enjoying Django (not
to mention I've barely scratched the surface of what it's capable of).

## Conclusion
I'm sorry that this wasn't a huge post and I don't have much to offer you this week outside of a recommendation to try the Django tutorial. In the coming weeks I'd
like to make a small web app or two using Django so I can get a better understanding of the framework and cement my opinion of it. I hope to see you back next week to, hopefully, talk
about some more Django. Thanks for reading!
