---
layout: post
title:  "CSS: Media Queries for Standard Devices"
date:   2015-04-06 17:00:00
categories: css
tags: css web
---

>Tested envrionment: ionic & chrome developer tool device simulator

If you're looking for a comprehensive list of media queries, [this repository](http://cssmediaqueries.com/overview.html) is a good resource.

iPhones
------

~~~
/* ----------- iPhone 4 and 4S ----------- */
/* Portrait */

@media only screen and (min-device-width: 320px) 
  and (max-device-height: 480px) 
  and (-webkit-min-device-pixel-ratio: 2) 
  and (orientation: portrait) {

}

/* ----------- iPhone 5 and 5S ----------- */
/* Portrait */

@media only screen and (min-device-width: 320px) 
  and (max-device-height: 568px) 
  and (-webkit-min-device-pixel-ratio: 2) 
  and (orientation: portrait) {

}

/* ----------- iPhone 6 ----------- */

/* ----------- iPhone 6 ----------- */
/* Portrait */

@media only screen 
  and (min-device-width: 375px) 
  and (max-device-height: 667px) 
  and (-webkit-min-device-pixel-ratio: 2) 
  and (orientation: portrait) {

}

/* ----------- iPhone 6+ ----------- */

/* Portrait */
@media only screen 
  and (min-device-width: 414px) 
  and (max-device-width: 736px) 
  and (-webkit-min-device-pixel-ratio: 3)
  and (orientation: portrait) { 

}

~~~
{: .language-css}
