---
title: "Making a Crappy Django App"
date: 2018-02-11
draft: false
---

## A Due Date App in Django
Code is available [here](https://github.com/aowens-21/due-date).

I'm sorry this blog post is late, which means there's another one coming tomorrow night. This week I focused on learning a bit more about
[Django](https://www.djangoproject.com/) by making an app that was sort of like the tutorial app I made last week. I decided to do something
that at least has the potential to be useful: a date-sorted to-do list.

## The App
Here's a picture of the homepage of the app:

<p align="center">
  <img src="/img/due-date-index.png">
</p>

While it's very primitive in terms of style, I think the building blocks are there. In Django especially it would be pretty easy to expand this
into a pretty interesting and useful application. What's also interesting is that it's insanely simple to sort the due dates using built-in Django functionality,
here's the entire function for the index page view:

```python
                  def index(request):
                      upcoming_assignments = Assignment.objects.filter(completed=False).order_by('due_date')
                      context = {'upcoming_assignments': upcoming_assignments}
                      return render(request, 'due/index.html', context)
```

All it's doing is storing all the assignment objects that aren't completed into a list and then sorting them the due date field. The view then stores
this into a dictionary called `context`, and that is passed to the page template which uses it to render the actual date. This brings us to
the template:

```HTML
                  <h1>Upcoming Assignments</h1>
                  <br />
                  {% if upcoming_assignments %}
                  	<ul>
                  	{% for assignment in upcoming_assignments %}
                  		<li><a href="{% url 'due:detail' assignment.id %}">{{ assignment.name }} - Due: {{ assignment.due_date }}</a></li>
                  	{% endfor %}
                  {% else %}
                  	<p>No assignments due, take a break!</p>
                  {% endif %}
                  <br />
                  <a class="btn btn-primary" href="{% url 'due:create_assignment' %}">Create Assignment</a>
```

As you can see, the template is pretty simple too. It's just Django's official template language interspersed with some HTML. You can see we're checking for the
`upcoming_assignments` list that pass in from the view, and if there aren't any we display a static message. This language is pretty flexible and I believe the context
dictionary can be as big as you need it to.

## Conclusion
There's a lot of other, similar code for the few other pages in the application that you can check out over on my GitHub. There's also a lot
of Django features I didn't use here, and I mean a lot. For example, instead of using functions to create views to handle responses, you can have
[class-based views](https://docs.djangoproject.com/en/2.0/topics/class-based-views/) which I think allows for more scalability. That is one of the things I really enjoy
about using Django. It seems like overkill for a really small project like this (and it probably is), but if you're building something big then the features
are pretty much there whenever you need them. I think I'm going to stick with Django for a couple more weeks, so the next few blog posts might also be Django related, but
after that I'm going to check out a smaller framework like [Flask](http://flask.pocoo.org/) or [Pyramid](https://trypyramid.com/). I think I need to dip my toes into the
basics before I dive back into something as huge as Django.
