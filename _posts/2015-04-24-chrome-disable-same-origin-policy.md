---
layout: post
title:  "Chrome: Disable same origin policy"
date:   2015-04-24 17:00:00
categories: chrome
tags: chrome
---

> [same orgiin policy](https://en.wikipedia.org/wiki/Same-origin_policy)

OSX
---

{% highlight sh %}
$ open -a Google\ Chrome --args --disable-web-security
{% endhighlight %}

Linux
---

{% highlight sh %}
$ google-chrome --disable-web-security
{% endhighlight %}

Also if you're trying to access local files for dev purposes like AJAX or JSON, you can use this flag too.

~~~
-–allow-file-access-from-files
~~~

Window
---

{% highlight bat%}
chrome.exe --disable-web-security
{% endhighlight %}
