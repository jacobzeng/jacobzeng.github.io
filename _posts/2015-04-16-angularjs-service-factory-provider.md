---
layout: post
title:  "angularjs: service, factory and provider"
date:   2015-04-16 22:05:40
categories: tools
tags: angularjs javascript
---

They all work the same way: by **running something once**, storing the value they get, and then cough up that same stored value when referenced through Dependency Injection.

{% highlight javascript %}
app.factory('a', fn);
app.service('b', fn);
app.provider('c', fn);
{% endhighlight %}

**The difference between the three is that:**

* `a`\'s stored value comes from running `fn`.
+ `b`'s stored value comes from newing `fn`.
+ `c`'s stored value comes from first getting an instance by newing `fn`, and then return a `$get` method of the instance.

which means, there’s something like a cache object inside angular, whose value of each injection is only assigned once, when they've been injected the first time, and where:

{% highlight javascript %}
cache.a = fn()
cache.b = new fn()
cache.c = (new fn()).$get()
{% endhighlight %}

This is why we use `this` in services, and define a `this.$get` in providers.

