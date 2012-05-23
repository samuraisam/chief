Chief
=====

A web framework written in Python using [Django][django] for creating web services.

It's similar in spirit to the likes of [Piston][piston] and [Tastyie][tastypie]. Chief strives to be more lightweight, simpler to used, and offer features beyond scaffolding HTTP methods in front of Django models. It is designed for the next generation of REST APIs and eschews outdated techniques.

**REST Conventions Followed:**

* Versioning is done per-resource using a kind of [Semantic Versioning][semver]
* Resources should not have a trailing slash or an extension
* Use of the `Accept` header to select version and content type
* Implement the necessary parts to enable [Cross-Origin Resource Sharing (CORS)][cors]

**Developer Conventions Followed:**

* It's up to you what kind of API you want to build
* Reasonable defaults are provided wherever possible
* Easy tasks should be easy - hard tasks totally do-able

**Some cool features:**

* Multiple authentication types per resource
* OAuth1.0a/2.0 and HTTP Basic Authentication implementations
* Very flexible URL routing options
* Easily support multiple versions of a resource

**Motivation**

I had been using [Piston][piston] for quite a while, loving it, but finally outgrew it while working on a larger project. In looking at all the other available "REST frameworks" nothing seemed to suit what I wanted. They all seemed focused on providing front ends to Django models. While useful for small, simple projects, grown up projects typically will outgrow their humble beginnings and require more sophisticated functionality rather quickly.

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

### Seperation of Concerns

#### Give a helping hand

### Versioning

#### Default Version

The default version for all resources is `1.0`. If no `version` attribute is specified it's assumed to be version `1.0` and will both respond to requests that ask for a version that does not exist, or requests that do not ask for a version at all.

#### Scheme

Handlers are versioned explicitly using class-level variables. Like this:

```python
class MyHandler(Handler):
	version = 1.1
```

Multiple versions may be specified for the same handler.

```python
class MyHandler_v11(Handler):
	version = 1.1

class MyHandler_v12(Handler):
	version = 1.2
```

### Content Types

#### Default Content Type

By default, if all else fails in trying to find a suitable emitter, Chief will emit a JSON response.

#### Emitters

Currently the only emitter that is shipped with Chief is JSON.

[django]: http://djangoproject.com
[piston]: https://bitbucket.org/jespern/django-piston/wiki/Home
[tastypie]: http://tastypieapi.org/
[semver]: http://semver.org/
[cors]: http://www.w3.org/TR/cors/