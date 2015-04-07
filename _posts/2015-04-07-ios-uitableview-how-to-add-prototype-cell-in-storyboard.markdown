---
layout: post
title:  "IOS UITableView: How to add prototype cell in storyboard"
date:   2015-04-07 17:00:00
categories: ios
tags: ios uitableview
---

>IOS SDK 8.2, Xcode Version 6.2

Sometimes, you want to define the UI of prototype cell in storyboard, it's very straightforward.

Define the ui what you want
-------

1. Select `Table View`, show `Attributes inspector`, find the `Prototype Cells`
![find prototype cells attribute]({{site.url}}/assets/How to add prototype cell in storyboard/5.png)

2. Set attribute `Prototype Cells` to 1, sure of that you can set more prototype cells if you want
![set prototype cells attribute to 1]({{site.url}}/assets/How to add prototype cell in storyboard/10.png)

3. Set attribute `Row Height` for more space to hold ui elements in cell
![set row hegiht]({{site.url}}/assets/How to add prototype cell in storyboard/20.png)

4. Select `Table View Cell`, set attribute `Identifler`, will use it latter
![set identifler]({{site.url}}/assets/How to add prototype cell in storyboard/15.png)

