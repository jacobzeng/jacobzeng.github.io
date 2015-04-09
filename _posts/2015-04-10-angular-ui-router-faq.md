---
layout: post
title:  "angular-ui-router: FAQ"
date:   2015-04-10 17:00:00
categories: angular-ui
tags: angular-ui ui-router javascript
---

Q: $state.go pass parameter with out any query params in url
===

1\. Define the params at $stateProvider
{% highlight javascript %}
$stateProvider.state(statename, {params: { 'someparam': undefined }}),
{% endhighlight %}

2\. When you want to pass params
{% highlight javascript %}
$state.go(statename, { someparam: "asd" });
{% endhighlight %}

3\. Access the params at CTRL method,  `$stateParams.someparam`

And what the console.log($stateParams) at that state says:  
Object {someparam: undefined} - when nothing sended  
Object {someparam: "asd"} - when transfered by state.go (2)

Also worked with : ui-sref="stateName({someparam:"asd"})"