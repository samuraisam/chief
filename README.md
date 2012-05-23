Chief
=====

A web framework written in Python using [Django][django] for creating web services.

It's similar in spirit to the likes of [Piston][piston] and [TastyPie][tastypie] but is much more lightweight and less focused on automatically scaffolding REST servies to serve as HTTP fronends to Django models.

**Conventions followed:**

* Versioning is done per-resource using [Semantic Versioning][semver]
* Resources should not have a trailing slash or an extension
* Clients select the desired vesrion of a resource and what content-type it should deliver via the `Accept` header using a vendor-specific content types
* Beyond that it's up to you what kind of API you want to build
* Reasonable defaults are provided wherever possible
* Easy tasks should be easy - hard tasks totally do-able

**Motivation**

I had been using [Piston][piston] for over a year on various projects and finally got fed up with the lack of updates and lock-in to a number of old-fashioned ways of building APIs. In looking at all the other available "REST frameworks" for Django and Python in general (though preferably using Django) they all seemed focused on providing front ends to Django models. While useful for small, simple projects, grown up projects typically will outgrow their humble beginnings and require more sophisticated functionality.

Gradually, while working on a larger REST application in Python using Django/Piston, I began to extract the bits I wanted changing from Piston into this framework. Therefore, there are some similarities between the two. Primarily in the concept of "Resources" and "Handlers". In fact, porting applications from Piston to Chief should be trivial in most cases.

**So what does this look like?**

Well, using a superficially simple example, the following will 

In `handlers.py`:
```python
from chief.handler import Handler

class EventHandler(Handler):
	def read(self):
		return {'id': 1, 'message': 'Hello!'}
```

In `urls.py`:
```python
from chief.resource import Resource
from chief.urls import Site

site = Site()
site.put(Resource('event'))

urlpatterns = site.patterns()
```


[django]: http://djangoproject.com
[piston]: https://bitbucket.org/jespern/django-piston/wiki/Home
[tastypie]: http://tastypieapi.org/
[semver]: http://semver.org/