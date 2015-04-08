---
layout: post
title:  "IOS: Remove default margin in ViewController"
date:   2015-04-07 17:00:00
categories: ios
tags: ios layout viewcontroller
---
>IOS SDK 8.2, Xcode Version 6.2

There are default margin in ViewController, sometimes it will make your UI broken.

![5.png]({{site.url}}/assets/IOS Remove default margin in ViewController/5.png)

Reason:
------
iOS8 SDK introduces a new concept: the “layout margins”. A new property has been added to the UIView class:

~~~
@property(nonatomic) UIEdgeInsets layoutMargins
~~~

These 4 values (the UIEdgeInsets fields) represent the margins of the view. Its subviews can now be positioned relative to these  margins.


Solution:
------

1. Click View which want to remove margin to parent view, select `Size inspector`, double click `Leading Space to: SuperView`
![10.png]({{site.url}}/assets/IOS Remove default margin in ViewController/10.png)

2. Uncheck the `Relative to margin` in attribute `Second Item` and the constraint will be relative to the left side of the view (not to the left margin of it)
![15.png]({{site.url}}/assets/IOS Remove default margin in ViewController/15.png)

3. Click View which want to remove margin to parent view, select `Size inspector`, double click `Trailing Space to: SuperView`
![20.png]({{site.url}}/assets/IOS Remove default margin in ViewController/20.png)

3. Check the `Relative to margin` in attribute `First Item` and the constraint will be relative to the right side of the view (not to the right margin of it)
![25.png]({{site.url}}/assets/IOS Remove default margin in ViewController/25.png)


No default margin now
------

![30.png]({{site.url}}/assets/IOS Remove default margin in ViewController/30.png)
